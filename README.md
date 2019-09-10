# fit3143-presentation

## pixel-based
each processor calculates RGB values for each pixel and writes the value at appropriate offset in the PPM file

```diff
root node
# start measuring the time and print init messages
+ MPI_File_open(MPI_COMM_WORLD, filename, MPI_MODE_RDWR | MPI_MODE_CREATE, MPI_INFO_NULL, &fp);
+ headerSize = sprintf(header, "P6\n %s\n %d\n %d\n %d\n", comment, iXmax, iYmax, MaxColorComponentValue);
+ MPI_File_write(fp, header, headerSize, MPI_CHAR, MPI_STATUS_IGNORE);

we wait for root to finish writing the header and broadcast
+ MPI_Barrier(MPI_COMM_WORLD);
+ MPI_Bcast(&headerSize, 1, MPI_INT, 0, MPI_COMM_WORLD);

# looping through each pixel

in the inner most loop, after computing RGB value for each pixel
+ MPI_File_write_at(fp, headerSize + (iY*iXmax + iX)*3, color, 3, MPI_CHAR, MPI_STATUS_IGNORE);

# after writing the image file

+ MPI_Barrier(MPI_COMM_WORLD);
+ MPI_File_close(&fp);

root
# start measuring the time and print completion messages
```


## row-based
three variations of row-based partition
### one row, parallel io


### batch of rows, parallel io
<img src="assets/batch_rows.png" width="60%">



### one row, root manages io


## conclusion
<img src="assets/comparisions.png" width="60%">
