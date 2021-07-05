# How to set background image for Flutter DataTable SfDataGrid

Set the background image to Flutter DataTable by wrapping the DataGrid inside the Container and set the color as transparent for rows and header. Then add the image to the image property of decoration box in Container.

## STEP 1

 Create the folder inside the application and add the image inside that folder. Also, add the image path in pubspec.yaml file. 

 ```xml
   assets:
    - image/DataGrid.png
```

## STEP 2 

Create EmployeeDataGridSource class extends with DataGridSource for mapping data to the SfDataGrid. Override the buildRow method and return the DataGridRowAdapter. You should set row background color as transparent using color property of DataGridRowAdapter.

```xml
class EmployeeDataGridSource extends DataGridSource {
  /// Creates the employee data source class with required details.
  EmployeeDataGridSource({required List<Employee> employeeData}) {
    _employeeData = employeeData
        .map<DataGridRow>((e) => DataGridRow(cells: [
              DataGridCell<int>(columnName: 'id', value: e.id),
              DataGridCell<String>(columnName: 'name', value: e.name),
              DataGridCell<String>(
                  columnName: 'designation', value: e.designation),
              DataGridCell<int>(columnName: 'salary', value: e.salary),
            ]))
        .toList();
  }

  List<DataGridRow> _employeeData = [];

  @override
  List<DataGridRow> get rows => _employeeData;

  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    return DataGridRowAdapter(
      color: Colors.transparent,
        cells: row.getCells().map<Widget>((e) {
      return Container(
        alignment: Alignment.center,
        padding: EdgeInsets.all(8.0),
        child: Text(e.value.toString()),
      );
    }).toList());
  }
}
```
 
## STEP 3

 Initialize the SfDataGrid with all the required properties. Set the headerColor as transparent using  SfDataGridThemeData.headerColor property. Wrap the SfDataGridThemeData inside the container and add the image to BoxDecoration.image property.

```xml
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: Container(
        decoration: BoxDecoration(
            image: DecorationImage(
                image: AssetImage("image/BackgroundImage.png"), fit: BoxFit.cover)),
        child: SfDataGridTheme(
          data: SfDataGridThemeData(headerColor: Colors.transparent),
          child: SfDataGrid(
            source: employeeDataGridSource,
            columnWidthMode: ColumnWidthMode.fill,
            columns: <GridColumn>[
              GridColumn(
                  columnName: 'id',
                  label: Container(
                      padding: EdgeInsets.all(16.0),
                      alignment: Alignment.center,
                      child: Text(
                        'ID',
                      ))),
              GridColumn(
                  columnName: 'name',
                  label: Container(
                      padding: EdgeInsets.all(8.0),
                      alignment: Alignment.center,
                      child: Text('Name'))),
              GridColumn(
                  columnName: 'designation',
                  label: Container(
                      padding: EdgeInsets.all(8.0),
                      alignment: Alignment.center,
                      child: Text(
                        'Designation',
                        overflow: TextOverflow.ellipsis,
                      ))),
              GridColumn(
                  columnName: 'salary',
                  label: Container(
                      padding: EdgeInsets.all(8.0),
                      alignment: Alignment.center,
                      child: Text('Salary'))),
            ],
          ),
        ),
      ),
    );
  }
```