|image0|

pip及Visual Studio编译一闪而过
==================================

一、pip警告
---------------

issue:

::

    SNIMissingWarning: An HTTPS request has been made, but the SNI (Subject Name Indication) extension to TLS is not available on this platform. This may cause the server to present an incorrect TLS certificate, which can cause validation failures. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#snimissingwarning. 
    InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning. 
    InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.

answer:

::

    `pip install pyopenssl ndg-httpsclient pyasn1`

二、Visual Studio编译一闪而过
---------------------------------

issue:

按F5出现dos窗口一闪而过

answer:

正确的应该是Ctrl+F5，F5是Debugging模式，在这个模式下，当程序运行结束后，窗口不会继续保持打开状态，而Ctrl+F5是
Start Without Debugging模式。或者在代码return 之前加上getchar()。

.. |image0| image:: http://p20tr36iw.bkt.clouddn.com/fu.png

