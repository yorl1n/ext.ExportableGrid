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

Possible improvements
-----------------------
As proposed @nbgo to identify exportable type, the type of column could be used. 
It will be helpful in cases, when appropriate column type is used for data displayment.

The following code snippets should be modified:

Replace
```javascript
getExportableColumnType: function (type) {
            return this.columnTypes[type] ? this.columnTypes[type] : this.defaultType;
        }
```

with

```javascript
getExportableColumnType: function (gridColumn) {
    if (Ext.grid.column.Number && gridColumn instanceof Ext.grid.column.Number) {
        return this.columnTypes['float'];
    }
    if (Ext.grid.column.Date && gridColumn instanceof Ext.grid.column.Date) {
        return this.columnTypes['date'];
    }
    if (Ext.grid.column.Boolean && gridColumn instanceof Ext.grid.column.Boolean) {
        return this.columnTypes['boolean'];
    }
    if (Ext.grid.column.Check && gridColumn instanceof Ext.grid.column.Check) {
        return this.columnTypes['boolean'];
    }
    return this.defaultType;
}
```

Replace marked with asterisk line
```javascript
if (!rec.isSummary && !rec.isGroupingHeader) {
                        if (param.xtype === 'templatecolumn') {
                            type = 'template';
                        } else if (param.exportConverter) {
                            type = 'converter';
                        } else if (!param.skipRenderer && param.renderer) {
                            type = 'renderer';
                        } else {
*                            type = this.getExportableColumnType(exportTask.exportableColumns[j]);
                        }
                    } else {
                        type = 's';
                    }
```

with
```javascript
type = rec.getField(param.dataIndex) ? this.getExportableColumnType(rec.getField(param.dataIndex).type) : this.defaultType;
```
