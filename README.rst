pymodaq_plugins_avantes
#######################

.. image:: https://img.shields.io/pypi/v/pymodaq_plugins_avantes.svg
   :target: https://pypi.org/project/pymodaq_plugins_avantes/
   :alt: 0.1

.. image:: https://readthedocs.org/projects/pymodaq/badge/?version=latest
   :target: https://pymodaq.readthedocs.io/en/stable/?badge=latest
   :alt: Documentation Status

.. image:: https://github.com/PyMoDAQ/pymodaq_plugins_avantes/workflows/Upload%20Python%20Package/badge.svg
   :target: https://github.com/PyMoDAQ/pymodaq_plugins_avantes
   :alt: Publication Status

.. image:: https://github.com/PyMoDAQ/pymodaq_plugins_avantes/actions/workflows/Test.yml/badge.svg
    :target: https://github.com/PyMoDAQ/pymodaq_plugins_avantes/actions/workflows/Test.yml


Authors
=======

* Bernhard Lang  (bernhard.lang@unige.ch)
* Eric Studemann (eric.studemann@unige.ch)


Instruments
===========

Viewer1D
++++++++

* **avantes**: control of an Avantes AvaSpec ULS2048CL EVO spectrometer

Installation instructions
=========================

* PyMoDAQ 5.0.1
* tested: Debian Linux 11/12, Windows 11
* Drivers / binaries needed from Avantes
   * Linux: libavs.so (in libavs_..._amd64.deb)
   * Windows: avaspecx64.dll (AvaspecX64DLL_...Setup_64bit.exe)
   * MacOS: libavs.dylib
   * any: avaspec.py


Linux
+++++

* Avantes publishes a Debian installation package libavs_..._amd64.deb. Run

  .. code-block::

    sudo pdkg -i libavs_..._amd64.deb

  to install its contents. Depending on the configuration of the Linux brand
  you're using, the file may end up in a directory where the dynamic linker
  doesn't find it. If that happens to you, you may also install the dynamic
  library by hand by copying it to a location where the dynamic linker will
  find it on your system. On Debian you may chose /usr/local/lib/avantes as
  "home" for the library. Copy the file libavs.so.whatever.version.number to
  that directory and run inside that directory

  .. code-block::

    sudo ln -s libavs.so.whatever.version.number libavs.so

* Create (using sudo and your favorite editor) the file

  .. code-block::

     /etc/udev/rules.d/90-avantes.rules

  and set its content to

  .. code-block::

     SUBSYSTEM=="usb", ATTRS{idVendor}=="1992", ATTRS{idProduct}=="<pid>", MODE="0666"

  where <pid> stands for the USB product ID of the device. It can ba found
  by calling lsusb -v and looking for Avantes in the output. For instance, 0669
  is the ID of the AvaSpec ULS2048CL, 0667 of the ASS216.

* Run

  .. code-block::

    sudo udevadm control --reload-rules

   and connect the spectrometer. It should now be recognised. In case it
   doesn't, carefully check the content of the udev rule file and reload it
   using the above command. Issuing

  .. code-block::

     sudo udevadm trigger

   forces the udev syatem to cycle through the initialisation process as
   if the connected devices would have been physically disconnected and
   reconnected again.


Windows
+++++++

* Execute AvaspecX64DLL_...Setup_64bit.exe. The installer will ask you for a
  password which you have to get from the AvaSpec Library Manual which you
  should have received from Avantes together with the device.
* The setup program will install and configure the device driver. Connect
  the device and check the device manager for an entry named
  "Avantes Spectrometers".
* Amongst other files, the installation has copied the library avaspecx64.dll
  to your disk. Find the location and either add the corresponding path your
  search path environment variable or copy the library to a folder where
  python will find it.
* A copy of that library is contained in this package. However, this ia not an
  official distribution. Avantes Inc. may change its contents. Using the copy
  contained here may therefore break other software delivered by Avantes Inc.
* Keep in mind also that a copy of that library alone is not sufficient. The
  device needs to be registered with the USB system. The setup program performs
  this task.


Continuation on any OS
++++++++++++++++++++++

* Inside a shell (i.e. Miniforge Prompt on Windows) run

  .. code-block::

    pip install pymodaq_plugins_avantes

* Start the dashboard and create a preset using the avantes plugin.
