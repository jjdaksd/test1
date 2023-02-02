```
import os
import shutil
from tqdm import tqdm

def get_directory_size(src, file_sizes):
    for item in os.listdir(src):
        s = os.path.join(src, item)
        if os.path.isdir(s):
            get_directory_size(s, file_sizes)
        else:
            file_sizes[s] = os.path.getsize(s)

def copy_directory(src, dst, pbar, copied_size, file_sizes):
    for item in os.listdir(src):
        s = os.path.join(src, item)
        d = os.path.join(dst, item)
        if os.path.isdir(s):
            if not os.path.exists(d):
                os.makedirs(d)
            copied_size = copy_directory(s, d, pbar, copied_size, file_sizes)
        else:
            if not os.path.exists(d) or (os.path.exists(d) and (os.path.getsize(d) != file_sizes[s])):
                with open(s, 'rb') as fsrc:
                    with open(d, 'wb') as fdst:
                        shutil.copyfileobj(fsrc, fdst, 1024*1024*10)
                copied_size += file_sizes[s]
                pbar.update(file_sizes[s])
    return copied_size

src_folder = '/path/to/src_folder'
dst_folder = '/path/to/dst_folder'

file_sizes = {}
get_directory_size(src_folder, file_sizes)

total_size = sum(file_sizes.values())
copied_size = sum(os.path.getsize(f) for f in (os.path.join(dirpath, f) for dirpath, dirnames, files in os.walk(dst_folder) for f in files))

with tqdm(total=total_size, initial=copied_size, unit='B', unit_scale=True, desc='Copying...') as pbar:
    copied_size = copy_directory(src_folder, dst_folder, pbar, copied_size, file_sizes)

```
