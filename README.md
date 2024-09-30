# How to build a dataset from OpenF1

### Operation 1 - f1-led-circuit-fetch-all-raw-data

1. Update main.rs to the correct session_id
2. Update main.rs to use the driver numbers for the drivers that participated in that session_id
3. ```rust cargo run```
4. You now how a full race dataset for that session_id

Note: Sometimes the server will flake and you'll get an Internal Server Error (500). In this case just run it again until you don't receive any errors.

### Opeartion 2 - f1-led-circuit

1. Move into the KiCad directory.
2. Identify and update the python script *get_x_y_from_origin...*
3. Open the pythong script editor in KiCad
3. Run the following

```python
exec(open("/Users/hott/eng/f1-led-circuit/kicad/x_y_led_coords_zandvoort/get_x_y_from_origin_20x20.py").read())
```
4. You now have the raw Designators, X, Y, and Angle data for the LEDs on the PCB

### Operation 3 - Copy files into f1-led-circuit-normalize-data

1. Copy the race raw dataset from Operation 1
2. Copy the led data from Operation 2

### Operation 4 - Modify the led data

1. Open the led data csv file and resave as an excel workbook.
2. Open the excel workbook.
3. Select the colums for x and y
4. Format them as numbers with 2 decimal places
5. Change the column "Designator" to "led_num"
6. Run the following VBA script to remove the leading "U" from column A.

```vba
Sub RemoveU()
    Dim cell As Range
    Dim ws As Worksheet
    
    ' Set the worksheet, change "Sheet1" to your sheet's name if needed
    Set ws = ThisWorkbook.Sheets("zandvoort_led_coordinates_20x20")
    
    ' Loop through the range A1:A218
    For Each cell In ws.Range("A1:A218")
        ' Remove the "U" from the cell value
        cell.Value = Replace(cell.Value, "U", "")
    Next cell
    
    MsgBox "The 'U' has been removed from the cells in the range A1:A218."
End Sub
```

7. Resave the workbook as a csv file with a different file name.

### Operation 5 - f1-led-circuit-normalize-data

1. Update main.rs to use the raw race data from Operation 1
2. Update main.rs to use the modified led data from Operation 4
3. ```rust cargo run```
4. You now have normalized x,y positions and led numbers

### Operation 6 - f1-led-circuit-nearest-neighbor

1. Copy the normalized data to the f1-led-circuit-nearest-neighbor directory
2. Change the headers x,y to x_led, y_led in the normalized data csv.
3. ```rust cargo run```


