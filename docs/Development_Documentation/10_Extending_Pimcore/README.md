# Extending Pimcore

When building solutions with Pimcore, normally one starts with configuring an object data model, 
create custom controller, actions and templates, creating documents and so on. Lots of things 
are possible without extending Pimcore itself. 
But depending on the desired result, sometimes it is necessary to extend default functionality
of Pimcore. 

For that Pimcore provides several ways for extension. Depending on the use case one or the other
way may fit best. 

Following a list of ways to extend Pimcore. See detail pages for additional information: 

* [**Add your own Dependencies and Packages**](./01_Add_Your_Own_Dependencies_and_Packages.md) for loading external libraries and functionalities 
 to be available in custom code. 
 
* Use [**Dependency Injection**](./03_Dependency_Injection.md) to overwrite Pimcore default models. 

* [**Constants File**](./05_Constants_File.md) for overwriting Pimcore constants like assets
 directory, temporary directory etc. 
 
* [**Parent Class for Objects**](./07_Parent_Class_for_Objects.md) to inject additional functionality
 to Pimcore object classes. 
 
* [**Hook into the Startup Process**](./09_Hook_into_the_Startup_Process.md) to attach event listeners, 
 register additional custom routes, etc. 
 
* [**Event API and Event Manger**](../10_Extending_Pimcore/11_Event_API_and_Event_Manager.md) for hooking into standard
 Pimcore functions like creating, updating, deleting elements etc. 
 
* Use [**Maintenance Mode**](./15_Maintenance_Mode.md) to show users a maintenance page when 
 changing system configurations. You also can create a custom maintenance page. 
 
* Add [**Custom Persistent Models**](./17_Custom_Persistent_Models.md) to save additional information. 

* [**Create Pimcore Plugins**](./13_Plugin_Developers_Guide/README.md) when you want to add complex and extensive functionality to Pimcore. 