pip install 做了什麼


一般安裝套件, pip 會把套件裡的檔案複製到site-packages/ 下面


import 機制


透過sys.path裡面的路徑去尋找尋找相對應的名稱



常見的套件管理工具: poetry, flit, setuptools

打造自己的套件 structure your package:




pkg
  |	__init__.py 告訴python這個資料夾是個套件
  |
  |____ module1.py
  |
  |____ module2.py   從1 import 2稱為intra-package reference (相對或絕對import)
  |
  |___ subpkg
	   |
         |__ __init__.py
         |
         |____ module3.py



Abs                                        |           Relative
----------------- ------------- -----------------------------------
from package.module1 import xxxx           |         from .module1 import xxx


建立使用絕對路徑，重構的時候才不會因為資料夾的轉變造成import error