# SelectOneAndManyWithHTML
These files enable one to use HTML along the EPiServer.Shell.ObjectEditing.ISelectionFactory implemenation. How to use modified editors you can check in ColorSelectionFactory.cs.

### How to

1. Place the editor files in your solution. Best fits in ~/ClientResources/Scripts/Editors. Otherwise you have to change paths.
2. Edit the NonEncodingSelectionEditor.js and NonEncodingCheckBoxListEditor.js and change prefix path in declaration to your site prefix. That you can found in root folder module.config.

    ##### Script

    ``` javascript
    define("YOUR_SITE_PREFIX/editors/NonEncodingSelectionEditor",
    define("YOUR_SITE_PREFIX/editors/NonEncodingCheckBoxListEditor",    
     ```
    ##### Config

    ```xml
    <dojo>    
      <paths>
        <add name="YOUR_SITE_PREFIX" path="Scripts" />
      </paths>
        …
    ```
    
3. Decorate and setup the property/ies.
    ```c#
    [SelectOne(SelectionFactoryType = typeof(ColorSelectionFactory))]
    public virtual string ColorDropDown { get; set; }

    [SelectMany(SelectionFactoryType = typeof(ColorSelectionFactory))]
    public virtual string ColorsCheckBoxes { get; set; }
    ```
    
4. Update you selection factory to use new non-encoding editors.
    
    ```c#
		public IEnumerable<ISelectItem> GetSelections(ExtendedMetadata metadata)
		{
			if (metadata.Attributes.Any(a => a.GetType() == typeof(SelectOneAttribute)))
			{
				metadata.ClientEditingClass = "alloy/editors/NonEncodingSelectionEditor";
			}
			else if (metadata.Attributes.Any(a => a.GetType() == typeof(SelectManyAttribute)))
			{
				metadata.ClientEditingClass = "alloy/editors/NonEncodingCheckBoxListEditor";
			}
			
		 …
     ```
### License
MIT
