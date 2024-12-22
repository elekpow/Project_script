*project script*


**Копирование файлов из одно директории в другую.**

- сокращение длины имени файла
параметр *max_length=150* определяет длину в 150 символов
- запись ошибок в файл error.txt

<details>
<summary> код скрипта: copy.py</summary>

```python

import os
import sys
import shutil
import time

def log_error(message):
    """Записывает сообщение об ошибке в файл error.txt и выводит на экран."""
    with open('error.txt', 'a') as error_file:
        error_file.write(message + '\n')
    print(message)

def error_on_dir(exc, dest_dir):
    # print('Error when trying to create DIR:', dest_dir)
    # print(exc)
    # print()
	message = f'Error when trying to create DIR: {dest_dir}\n{exc}\n'
    log_error(message)

def error_on_file(exc, src_path):
    # print('Error when trying to copy FILE:', src_path)
    # print(exc)
    # print()
    message = f'Error when trying to copy FILE: {src_path}\n{exc}\n'
    log_error(message)
	
def shorten_filename(filename, max_length=150):
    """Сокращает имя файла до max_length символов, сохраняя расширение."""
    name, ext = os.path.splitext(filename)
    if len(name) > max_length:
        name = name[:max_length]
    return name + ext
	
def shorten_directory_name(dir_name, max_length=150):
    """Сокращает имя каталога до max_length символов."""
    if len(dir_name) > max_length:
        return dir_name[:max_length]  # Обрезаем имя каталога
    return dir_name	
	
def copydir(source, dest):
    """Copy a directory structure overwriting existing files"""
    total_files_copied = 0
    total_size_copied = 0

    start_time = time.time()

    for root, dirs, files in os.walk(source):
        rel_path = root.replace(source, '').lstrip(os.sep)
        dest_dir = os.path.join(dest, rel_path)

        dest_dir = shorten_directory_name(dest_dir)


        try:
            os.makedirs(dest_dir)
        except OSError as exc:
       name = name[:max_length]
    return name + ext


def copydir(source, dest):
    """Copy a directory structure overwriting existing files"""
    total_files_copied = 0
    total_size_copied = 0

    start_time = time.time()

    for root, dirs, files in os.walk(source):
        rel_path = root.replace(source, '').lstrip(os.sep)
        dest_dir = os.path.join(dest, rel_path)

        try:
            os.makedirs(dest_dir)
        except OSError as exc:
            if 'file exists' not in str(exc).lower():
                error_on_dir(exc, dest_dir)

        for each_file in files:
            if each_file.startswith('~$') or each_file == 'Thumbs.db':
               # print('Skipping file: {}'.format(each_file))
                continue

            src_path = os.path.join(root, each_file)
            shortened_file_name = shorten_filename(each_file)
            # dest_path = os.path.join(dest_dir, shortened_file_name)
            dest_path = os.path.join(dest_dir, each_file)

            if os.path.exists(dest_path):
               # print('File already exists, skipping: {}'.format(dest_path))
                continue

            try:

                error_on_dir(exc, dest_dir)

        for each_file in files:
            if each_file.startswith('~$') or each_file == 'Thumbs.db':
               # print('Skipping file: {}'.format(each_file))
                continue

            src_path = os.path.join(root, each_file)
            shortened_file_name = shorten_filename(each_file)
            # dest_path = os.path.join(dest_dir, shortened_file_name)
           dest_path = os.path.join(dest_dir, each_file)

            if os.path.exists(dest_path):
               # print('File already exists, skipping: {}'.format(dest_path))
                continue

            try:
                shutil.copyfile(src_path, dest_path)
                total_files_copied += 1
               # print('Copied file: {}'.format(each_file))
            except Exception as exc:
                error_on_file(exc, src_path)

    end_time = time.time()
    elapsed_time = end_time - start_time

    minutes = int(elapsed_time // 60)
    seconds = int(elapsed_time % 60)

   # print('Total files copied: {}'.format(total_files_copied))
   # print('Time taken for copying: {} minutes and {} seconds'.format(minutes, seconds))

if __name__ == '__main__':
    arg = sys.argv
    if len(arg) != 3:
        print('USAGE: python copy.py SOURCE DESTINATION')
    else:
        copydir(arg[1], arg[2])
			
```	

</details>


**копирование по списку**


Прочитать файл list_files.txt и сохранить имена файлов в список.


`python copy.py SOURCE DESTINATION file_list.txt`



• SOURCE — путь к директории-источнику (например, C:path\tosource или /path/to/source).
• DESTINATION — путь к директории назначения (например, C:path\todestination или /path/to/destination).
• file_list.txt — имя файла со списком файлов.


Пример содержимого file_list.txt:

```
   file1.txt
   subdir/file2.txt
   image.png
```


<details>
<summary> код скрипта: copy_list.py</summary>

```python

import os
import sys
import shutil
import time

def log_error(message):
    """Записывает сообщение об ошибке в файл error.txt и выводит на экран."""
    with open('error.txt', 'a') as error_file:
        error_file.write(message + '\n')
    print(message)

def error_on_dir(exc, dest_dir):
    message = f'Error when trying to create DIR: {dest_dir}\n{exc}\n'
    log_error(message)

def error_on_file(exc, src_path):
    message = f'Error when trying to copy FILE: {src_path}\n{exc}\n'
    log_error(message)

def shorten_filename(filename, max_length=150):
    """Сокращает имя файла до max_length символов, сохраняя расширение."""
    name, ext = os.path.splitext(filename)
    if len(name) > max_length:
        name = name[:max_length]
    return name + ext
 
def shorten_directory_name(dir_name, max_length=150):
    """Сокращает имя каталога до max_length символов."""
    if len(dir_name) > max_length:
        return dir_name[:max_length]  # Обрезаем имя каталога
    return dir_name 

def load_file_list(file_path):
    """Загружает список файлов из файла."""
    try:
        with open(file_path, 'r') as f:
            return [line.strip() for line in f if line.strip()]
    except Exception as e:
        log_error(f"Error reading file list: {e}")
        return []

def copy_files(dest, file_list):
    """Копирует указанные файлы в dest."""
    total_files_copied = 0

    start_time = time.time()

    for each_file in file_list:
        src_path = each_file  # Используем путь файла из списка

        if not os.path.isfile(src_path):
            log_error(f"File does not exist: {src_path}")
            continue

        rel_path = os.path.dirname(src_path)
        dest_dir = os.path.join(dest, shorten_directory_name(rel_path))

        try:
            os.makedirs(dest_dir, exist_ok=True)
        except OSError as exc:
            error_on_dir(exc, dest_dir)

        shortened_file_name = shorten_filename(os.path.basename(src_path))
        dest_path = os.path.join(dest_dir, shortened_file_name)

        if os.path.exists(dest_path):
            log_error(f"File already exists, skipping: {dest_path}")
            continue

        try:
            shutil.copyfile(src_path, dest_path)
            total_files_copied += 1
        except Exception as exc:
            error_on_file(exc, src_path)

    end_time = time.time()
    elapsed_time = end_time - start_time

    minutes = int(elapsed_time // 60)
    seconds = int(elapsed_time % 60)

    # print('Total files copied: {}'.format(total_files_copied))
    # print('Time taken for copying: {} minutes and {} seconds'.format(minutes, seconds))

if __name__ == '__main__':
    arg = sys.argv
    if len(arg) != 3:
        print('USAGE: python copy.py DESTINATION LIST_FILE')
    else:
        destination_dir = arg[1]
        list_file_path = arg[2]

        files_to_copy = load_file_list(list_file_path)
        copy_files(destination_dir, files_to_copy)


```

</details>


**обработка файла error.txt**

из файла error.txt нужно получить file_list.txt


<details>
<summary> code extract_errors.py</summary>

```python
import os

# Определяем имена файлов
input_file = 'error.txt'
output_file = 'file_list.txt'

# Открываем файл error.txt для чтения
with open(input_file, 'r') as infile:
    # Читаем все строки из файла
    lines = infile.readlines()

# Открываем файл output_text.txt для записи
with open(output_file, 'w') as outfile:
    # Инициализируем счетчик строк
    line_count = 0
    
    # Проходим по каждой строке
    for line in lines:
        # Проверяем, начинается ли строка с нужного текста
        if line.startswith('Error when trying to copy FILE: '):
            # Извлекаем текст после префикса
            file_path = line[len('Error when trying to copy FILE: '):].strip()
            # Разбиваем путь на части
            path_parts = file_path.split(os.sep)
            # Оборачиваем в одинарные кавычки части, содержащие пробелы
            quoted_parts = [f"'{part}'" if ' ' in part else part for part in path_parts]
            # Собираем путь обратно
            quoted_path = os.sep.join(quoted_parts)
            # Записываем в output_text.txt
            outfile.write(quoted_path + '\n')
            # Увеличиваем счетчик строк
            line_count += 1

# Выводим количество записанных строк
print(f'Результаты записаны в {output_file}')
print(f'Количество обработанных строк: {line_count}')

```

</details>