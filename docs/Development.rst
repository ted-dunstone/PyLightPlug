==================
Plugin Development
==================

------------
Introduction
------------

There are many ways of developing plugin systems, and every project has different requirements. 
Some requirements however that are universal (particluarly for Scientific apps) include: 


* Auto registration of new plugins and the automatic update during development
* Derivation of more complex plugins from simpler ones
* Specify parameters that can be passed to the plugin
* Have the plugin manager determine which plugins can be used at any time
* Allow for additional functions easily later
* Provide good documentation on functionality

PyLightPlug uses the metaclass plugins pattern to achieve a lightweight extensible plugin (See [#metaclass_link]_).
It is initially centered around providing a simple discovery mechanism to ensure the correct plugin can be chosen dependant on requirements.

.. [#metaclass_link] The following has a good decription of a metaclass plugin implemtation http://effbot.org/zone/metaclass-plugins.htm
The plugin framework x


------------
Installation
------------


Clone the git repository::

    git clone git://github.com/ted-dunstone/PyLightPlug.git


Then run the python setup install::

    setup install


-----------------------
Writing a Simple Plugin
-----------------------

A plugin system achieves nothing on its own and so does not exist outside of the domain in which it is used. 
As an example of the use of PyLightPlug we will focus on a scientifc application which calculates the impact
of disasters. This is the domain in which it is was first developed (`see Risiko <http://www.riskinabox.org>`_)


Our first plugin we want to calculate a simple impact by multiplying the severity of hazard (i.e. the 
amount of ground shaking) by the exposure (i.e. the number of people in that area). e.g.::

    Impact =  Exposure x Hazard


As the first step we need to define the plugin class.::

    class EarthquakeFatalityFunction(PyLightPlug.FunctionProvider):
          pass

Every plugin must be subclassed from PyLightPlug.FunctionProvider. This is the 
method of registration for the plugin and allows the Plugin Manager to know what plugins are
available.

The parameters required to run the plugin, and indeed all parameters specific to the plugin,
are defined in the doc string of the class::

    class EarthquakeFatalityFunction(FunctionProvider):
    	"""Risk plugin for earthquake damage

    	:author Haji

    	:param requires category=="hazard" and subcategory.startswith("earthquake") and layerType=="raster"
    	:param requires category=="exposure" and subcategory.startswith("population") and layerType=="raster"
    	"""

This tells the PyLightPlug manager that this plugin requires inputs of

* category of 'hazard', with a subcategory of 'earthquake' and it must be a layerType of 'Raster'
* category of 'exposure', with a subcategory of 'earthquake' and it must be a layerType of 'Raster'

.. note:: Lines can be broken using the line continuation character '\' at the end of a line

Each plugin must define a run method which is the plugin execution code.::

    @staticmethod
    def run(input):
        """Risk plugin for earthquake fatalities

        Input
          inputs: Specifies a dictionary containing the input paramaters for the plugin
        """
        E=input['exposure']
        H=input['hazard']
        scale_constant=input('scale_constant')
        
        # Calculate impact
        Impact =  E * H * scale_constant

        # Return
        return Impact


The parameters are passed in as a dictionary. It is up to the framework to populate
the dictionary correctly in this case with keys containing relavent data for the exposure and hazard.

At the end of the function the calculated impact is returned.

------------------------
Using the Plugin Manager
------------------------

When building your plugin manager the following functions are exposed by PyLightPlug::
        
    def get_function(name):
        """Retrieves a plugin based on it's name
        """

    def pretty_function_name(func):
        """ Return a human readable name for the function
        if the function has a func.plugin_name use this
        otherwise turn underscores to spaces and Caps to spaces """
        
    def requirements_collect(func):
        """ Collect the requirements from the plugin function doc
        The requirements need to be specified using
          :param requires <valid pythhon expression>
        The layer keywords are put into the local name space
        each requires should be on a new line    
        returns the strings for the python exec

        Example of valid requires
        :param requires category=="impact" and subcategory.startswith("population")
        """
    
    def requirement_check(params, require_str, verbose=False):
        """Checks a dictionary params against the requirements defined
        in require_str. Require_str must be a valid python expression
        and evaluate to True or False"""
    
    def requirements_met(func, params, verbose=False):
        """Checks to see if the plugin can run based on the requirements
           specified in the doc string"""
    

The sequence of calls for the pyPluginManager is to use the requirements met function to determine
which plugins can run by passing a dictionary  `params` to the discovered plugins. Once the plugins
the can run have been discovered `get_function` can be called to obtain a handle to the plugin. The 
plugin can then be executed using::
  
    my_plugin = get_function('EarthquakeFatalityFunction')
    input_params = dict(category = 'hazard', subcategory='....')
    if requirement_met(my_plugin,params = input_params):
        my_plugin.run(input_params)
    
    

