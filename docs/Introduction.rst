============
Introduction
============

.. topic:: PyLightPlug

  A light weight python plugin system for scientific applications 

--------
Rational
--------

Those building scientific applications often require a way to extend application functionality, whilst sheltering the user from needing to know too much about the underlying application design. Plugin systems are usually designed to provide a way to abstract and extend functionality - and need a simple way of registering and ieasily maintained documentation. PyLightPlug uses meta classes and doc string introspection that is tailored for scientific applications. It aims to provides the following benefits:

#. Documentation: A structured doc string is used as part of the specification for the plugin. The format follows the Sphinx standard and hence provides well documented user information about the plugin. The same information is also is used to determine if the plugin can run (requirements filtering) as well as other display related attributes.
#. Requirements Filtering: Requirements are filtered based on one or more evaluated commands embedded in the doc string using a sphinx like format. These are used to determine if a plugin can run based on meta-data provided as dictionaries by the plugin manager.
#. Data Handing: A method of handling collections of scientific data based on flickr style tags.
#. Post Processing Decoration: A flexible decoration post processor to customize the styling of any output.
#. Debugging: Plugins can be easily debugged and tested via a command line interface. This ensures that plugin errors can be handled appropriately and also that changes to plugins are dynamically refreshed without requiring an application restart.
#. Rest Interface: An infrastructure that includes the batteries for using the plugin with web infrastructure.
#. Security:  The prevention of malicious plugin code by restricting the execution environment
#. Sharing: The ability to easily share developed plugins.
#. Light-Weight: The library is designed to be a compact and well documented

-------
History
-------

This module was developed as part of the Risiko project. Risiko is a Python/Django application that will model impacts of different hazard events on population or infrastructure using distributed spatial hazard and exposure data hosted using spatial data management frameworks from OpenGeo. This will allow local authorities to undertake generalimpact modelling to better prepare their local communities for the impact of natural disasters. (www.riskinabox.org)
