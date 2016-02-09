# ext.ExportableGrid
Wrapper over [ExtJs's](https://www.sencha.com/products/extjs/#overview) [grid](http://docs.sencha.com/extjs/6.0/6.0.1-classic/#!/api/Ext.grid.Panel) to make grid exportable to xlsx format. Supports ExtJs 5 and ExtJs 6. Example can be found on sencha's [fiddle](https://fiddle.sencha.com/#fiddle/t37). Supports composite headers, summary feature and grouping feature.

Dependencies
------------
This component uses two opensource libraries([FileSaver](https://github.com/eligrey/FileSaver.js) and [jszip](https://github.com/Stuk/jszip)), so they should be imported to the project.

ExportableGrid's specific properties
--------------------------
- **xlsTitle** - name of exported file + will be shown in excel as a table header if **xlsShowHeader** is set to true.
- **xlsHeaderColor** - backgound color of the headers. RGB without leading #.
- **xlsGroupHeaderColor** - backgound color of the grouping headers. RGB without leading #.
- **xlsSummaryColor** - backgound color of the summary section. RGB without leading #.
- **xlsShowHeader** - just a marker, whether a first row title should be generated.

Additional parameters for columns
---------------------------------
- **exportable** - `true/false` - defines whether this column should be exported. 
- **exportConverter** - function that shoud be applyed to value on export. Returns modified value. If **exportConverter** is specified, then renderer will be skiped and exportConverter function will be used on export. 
- **skipRenderer** - `true/false` - set to true to skip using renderer on export for this column. 
- **exportNumberFormat** - `<#,##.00>` - valid excel's number format.

