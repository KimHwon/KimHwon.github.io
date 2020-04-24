---
title: pdfrw를 사용한 PDF 업무 자동화
date: 2020-04-24
tags:
  - Python
  - PDF
  - TaskAutomation
keywords:
  - Python
  - PDF
  - TaskAutomation
  - 업무자동화
---

PDF 폼을 채우는 업무를 받았습니다. 대략 600 rows 쯤 되는 데이터를 분류해서 PDF에 채워 넣는 업무였습니다. <br>
당연히 손으로 일일히 채우기는 불편하기 때문에 Python을 사용해서 자동화를 해보았습니다.

구글링을 해보니 이미 PDF를 다룰 수 있는 다양한 패키지들을 배포하고 있었습니다. 그 중 `pdfrw`를 사용해서 만들어보았습니다.

# Install
```bash
pip install pdfrw   # Install with pip
conda install -c conda-forge pdfrw  # or with conda
```

# Codes

```python
# pdf.py
import pdfrw

def fill_pdf(input_pdf_path, output_pdf_path, data_dict):
	template_pdf = pdfrw.PdfReader(input_pdf_path)

	annotations = template_pdf.pages[0]['/Annots']
	for annotation in annotations:
		if annotation['/Subtype'] == '/Widget':
			if annotation['/T']:
				key = annotation['/T'][1:-1]

				if key in data_dict.keys():
					annotation.update(pdfrw.PdfDict(AP=f'{key}', V=f'{data_dict[key]}'))

	pdfrw.PdfWriter().write(output_pdf_path, template_pdf)

def fill_pdfs(input_pdf_path, output_pdf_path, data_dict_list):
	if not isinstance(data_dict_list, list):
		raise Exception('Error: Argument `data_dict_list` must be list.')

	template_pdf = pdfrw.PdfReader(input_pdf_path)

	for page in range(len(template_pdf.pages)):
		annotations = template_pdf.pages[page]['/Annots']

		for annotation in annotations:
			if annotation['/Subtype'] == '/Widget':
				if annotation['/T']:
					key = annotation['/T'][1:-1]

					if key in data_dict_list[page].keys():
						annotation.update(pdfrw.PdfDict(AP=f'{key}', V=f'{data_dict_list[page][key]}'))

	pdfrw.PdfWriter().write(output_pdf_path, template_pdf)

def merge_pdf(pdf_path_list, output_pdf_path):
	writer = pdfrw.PdfWriter()

	for pdf_path in pdf_path_list:
		pdf = pdfrw.PdfReader(pdf_path)
		writer.addpages(pdf.pages)

	writer.write(output_pdf_path)
```
PDF의 Field의 이름을 key, 채울 내용을 value로 하는 dictionary를 `raw_data`로 받아서 폼을 완성합니다.
`fill_pdf`에서 페이지 내에 있는 모든 annotation을 순회하면서 key가 존재하면, value를 넣고 update합니다.

`fill_pdfs`는 여러 페이지의 PDF문서를 채워 넣을 때 사용합니다. `raw_data_list`는 `raw_data`들의 배열로, 배열의 인덱스가 페이지의 인덱스에 대응됩니다.

그리고 저는 1장짜리 템플릿 PDF문서와 같은 포맷으로 데이터를 채워야했기 때문에, `merge_pdf`로 템플릿 PDF문서끼리 여러 번 merge하여 복사했습니다.


```python
# main.py
from pdf import fill_pdf, fill_pdfs, merge_pdf
import json
from math import ceil

RAW_DATA = 'data.json'

PDF_TEMPLATE_PATH = 'template.pdf'
PDF_OUTPUT_PATH = 'output.pdf'


with open(RAW_DATA) as json_file:
    raw_data = json.load(json_file)


max_page = int(ceil(len(raw_data) / 5.0))  # 1 page당 5 rows

template_path_list = [PDF_TEMPLATE_PATH for x in range(max_page)]
merge_pdf(template_path_list, 'temp.pdf')


data_dict_list = []
data_dict = {}
for idx, data_row in enumerate(raw_data):
    for key in data_row:
        data_dict[f'{key}-{(idx+1) % 5}'] = data_row[key]

    if (idx+1) % 5 == 0 or idx == len(appointment_sorted)-1:
        data_dict_list.append(data_dict)


fill_pdfs('temp.pdf', PDF_OUTPUT_PATH, data_dict_list)
```
정의한 함수들을 사용해서 완성한 코드입니다. JSON파일에서 raw data를 읽어와서 1페이지 당 5 rows 씩 나눠서 입력을 했습니다. <br>
제 경우에는 Field 이름이 `name-index`꼴이라 `f'{key}-{(idx+1) % 5}'`로 지정했습니다. 상황에 맞게 for문을 고치면 될 것 같습니다.


```python
# naming.py
import pdfrw

def fill_name_pdf(input_pdf_path, output_pdf_path):
    template_pdf = pdfrw.PdfReader(input_pdf_path)

	annotations = template_pdf.pages[0]['/Annots']
	for annotation in annotations:
		if annotation['/Subtype'] == '/Widget':
			if annotation['/T']:
				key = annotation['/T'][1:-1]

                # Update every fields.
				annotation.update(pdfrw.PdfDict(AP=f'{key}', V=f'{key}'))

	pdfrw.PdfWriter().write(output_pdf_path, template_pdf)
```
만약 PDF의 Field의 이름을 모른다면 위와 같은 코드로 확인해볼 수 있습니다.
폼의 내용으로 해당하는 Field의 이름을 채웁니다.