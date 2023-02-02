```
import os
import shutil

def copy_directory(src, dst):
    for item in os.listdir(src):
        s = os.path.join(src, item)
        d = os.path.join(dst, item)
        if os.path.isdir(s):
            if not os.path.exists(d):
                os.makedirs(d)
            copy_directory(s, d)
        else:
            if not os.path.exists(d) or (os.path.exists(d) and (os.path.getsize(d) != os.path.getsize(s))):
                with open(s, 'rb') as fsrc:
                    with open(d, 'wb') as fdst:
                        shutil.copyfileobj(fsrc, fdst, 1024*1024*10)

src_folder = r''
dst_folder = r''


copy_directory(src_folder, dst_folder)





```
