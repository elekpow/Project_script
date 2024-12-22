*project script*


```python

import os
import sys
import shutil
import time

def error_on_dir(exc, dest_dir):
    print('Error when trying to create DIR:', dest_dir)
    print(exc)
    print()

def error_on_file(exc, src_path):
    print('Error when trying to copy FILE:', src_path)
    print(exc)
    print()

def shorten_filename(filename, max_length=150):
    """Сокращает имя файла до max_length символов, сохраняя расширение."""
    name, ext = os.path.splitext(filename)
    if len(name) > max_length:
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
            dest_path = os.path.join(dest_dir, shortened_file_name)

           # dest_path = os.path.join(dest_dir, each_file)

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
            dest_path = os.path.join(dest_dir, shortened_file_name)

           # dest_path = os.path.join(dest_dir, each_file)

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