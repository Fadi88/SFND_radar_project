# quick look into the SFND radar project

## Selection of training, guard cells and offset

for the number of training and guard cells, same value used in the 1D CFAR was chosen and also gave goos results so
remaind as previously chosen in both direction

the offset however was a bit tricky/easy in the same time that is because it was really senstive to the values chosen 
as well the output in th RDM array was either a 0 or 1
```matlab
Tr = 12;
Tv = 12;

Gr = 4;
Gv = 4;

offset = 2;
```

## Supressing edges values

it was quite easy to do because the RDM was set to zeros or 1, so all the indices that had other values could be chosen
and set to zero 

```matlab
outliers_idx = RDM~=0 & RDM~=1;
RDM(outliers_idx) = 0;
```

## 2D CFAR steps
the tricky part was taking care of the indices.

- Slide the cell under test across the complete matrix. ensuring the CUT has margin for Training and Guard cells from the edges.
- Get noise sum for CUT using db2pow function.
- Average the summed values for all of the training cells used. After averaging convert it back to logarithmic using pow2db.
- Further use the offset to it to determine the threshold.
- Compare the signal under CUT against this threshold.
  - If the CUT level > threshold assign it a value of 1, else equate it to 0.
