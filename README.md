# SelectOneAndManyWithHTML
These files are Episerver CMS editors for EPiServer.Shell.ObjectEditing.SelectOneAttribute and EPiServer.Shell.ObjectEditing.SelectManyAttribute that do not encode HTML so they enable one to use HTML along the EPiServer.Shell.ObjectEditing.ISelectionFactory implementation.

### How to

1. Place the files in your solution in ~/ClientResources/Scripts/Editors.
2. Edit the NonEncodingSelectionEditor.js and NonEncodingCheckBoxListEditor.js and change prefix path in declaration to your site prefix. That you can found in root folder module.config.

    ##### Script

    ``` javascript
    define("YOUR_SITE_PREFIX/Editors/NonEncodingSelectionEditor",
    define("YOUR_SITE_PREFIX/Editors/NonEncodingCheckBoxListEditor",    
     ```
    ##### Config

    ```xml
    <dojo>    
      <paths>
        <add name="YOUR_SITE_PREFIX" path="Scripts" />
      </paths>
        …
    ```
    
3. Decorate the property/ies.
    ```c#
    [SelectOne(SelectionFactoryType = typeof(YOUR_SelectionFactory))]
    public virtual string DropDown { get; set; }

    [SelectMany(SelectionFactoryType = typeof(YOUR_SelectionFactory))]
    public virtual string CheckBoxes { get; set; }
    ```
    
4. Update your selection factory to use non-encoding editors.
    
    ```c#
    public IEnumerable<ISelectItem> GetSelections(ExtendedMetadata metadata)
    {
      if (metadata.Attributes.Any(a => a.GetType() == typeof(SelectOneAttribute)))
      {
        metadata.ClientEditingClass = "YOUR_SITE_PREFIX/Editors/NonEncodingSelectionEditor";
      }
      else if (metadata.Attributes.Any(a => a.GetType() == typeof(SelectManyAttribute)))
      {
        metadata.ClientEditingClass = "YOUR_SITE_PREFIX/Editors/NonEncodingCheckBoxListEditor";
      }
      
      return new[]
      {
        new SelectItem{Value = null, Text = "<img src=\"https://tinyurl.com/y3qapon2\">"},
      …
     ```
### License
Available via Academic Free License >= 2.1 OR the modified BSD license.
