


- [distutils](https://docs.python.org/3.8/library/distutils.html#module-distutils)

  distutils is the original build and distribution system first added to the Python standard library in 1998. While direct use of distutils is being phased out, it still laid the foundation for the current packaging and distribution infrastructure, and it not only remains part of the standard library, but its name lives on in other ways (such as the name of the mailing list used to coordinate Python packaging standards development).

- [setuptools](https://setuptools.readthedocs.io/en/latest/)

  setuptools is a (largely) drop-in replacement for distutils first published in 2004. Its most notable addition over the unmodified distutils tools was the ability to declare dependencies on other packages. It is currently recommended as a more regularly updated alternative to distutils that offers consistent support for more recent packaging standards across a wide range of Python versions.

- [wheel](https://wheel.readthedocs.org/)

  wheel (in this context) is a project that adds the bdist_wheel command to distutils/setuptools. This produces a cross platform binary packaging format (called “wheels” or “wheel files” and defined in PEP 427) that allows Python libraries, even those including binary extensions, to be installed on a system without needing to be built locally.

