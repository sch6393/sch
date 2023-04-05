Json
===

### 참고
* [JSON 검증 사이트](../../etc/README.md#json-검증-체크-사이트)

<br>

### 기본 변환 형식
```py
import json

# print('exec')
# print(array_json)
# print(type(array_json))
'''
[{'a': '0000', 'b': '----------------------------', 'script': '{\n  "c": "C:\\\\Test.py",\n  "d": "5"\n}'}]
<class 'list'>
'''
# print(array_json[0])
# print(type(array_json[0]))
'''
{'a': '0000', 'b': '----------------------------', 'script': '{\n  "c": "",\n  "d": "5"\n}'}
<class 'dict'>
'''
strtmp = json.dumps(array_json[0])
# print(strtmp)
# print(type(strtmp))
'''
{"a": "0000", "b": "----------------------------", "script": "{\n  \"c\": \"C:\\\\Test.py\",\n  \"d\": \"5\"\n}"}
<class 'str'>
'''
dicttmp = json.loads(strtmp)
# print(dicttmp)
# print(type(dicttmp))
'''
{'a': '0000', 'b': '----------------------------', 'script': '{\n  "c": "C:\\\\Test.py",\n  "d": "5"\n}'}
<class 'dict'>
'''
# str_script = dicttmp['script']
str_script = dicttmp.get('script')
# print(str_script)
# print(type(str_script))
'''
{
    "c": "C:\\Test.py",
    "d": "5"
}
<class 'str'>
'''

dict_script = json.loads(str_script)
# print(dict_script.get('c')) # C:\\Test.py
# print(dict_script.get('d')) # 5
```

<br>

### 변환함수
```py
# Python 객체를 JSON 파일로 저장
dump()

# Python 객체를 JSON 문자열로 변환
dumps()

# JSON 파일을 Python 객체로 가져옴
load()

# JSON 문자열을 Python 객체로 변환
loads()
```

<br>

### 관련 에러
* [TypeError: the JSON object must be str, bytes or bytearray, not dict](../error/type.md#the-json-object-must-be-str-bytes-or-bytearray-not-dict)
* [TypeError: the JSON object must be str, not 'TextIOWrapper'](../error/type.md#the-json-object-must-be-str-not-textiowrapper)
* [AttributeError: 'str' object has no attribute 'read'](../error/attribute.md#str-object-has-no-attribute-read)

<br>
