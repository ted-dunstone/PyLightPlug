==============
Plugin Manager
==============

The plugin manager keeps track of all the plugins and searching, requirements checking and retrieval functions. All communications with the plugin are achieved through a dictionary.

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
plugin can then be executed using the following type of call::
  
    my_plugin = get_function('EarthquakeFatalityFunction')
    input_params = dict(category = 'hazard', subcategory='....')
    if requirement_met(my_plugin,params = input_params):
        my_plugin.run(input_params)
    
    
-------------------------------
Getting a list of valid plugins
-------------------------------

To get a list of plugins that can execute for a given context (i.e. from a menu or selection box) all available plugins must be checked to see if the requirements are met::

    input_params = dict(category = 'hazard', subcategory='....')
    my_plugin = [get_function(name) 
    for name in get_all_functions()
        if requirement_met(my_plugin,params = input_params)]
