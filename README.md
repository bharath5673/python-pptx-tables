
# Pptx Tables


#### Use pptx_tables to create tables more easily through python-pptx.


## Features


- Provide data formatted as a list of lists or a list of dictionaries

- Provide custom headers

- Provide custom sort order on columns

- Set columns width

- Set font size

- Set cell alignment

- Set the row height


Samples
=======

Create a table of data on a slide
---------------------------------
```
from pptx_tables import PptxTable

data1 = [[0, 1, 2],
         [3, 4, 5],
         [6, 7, 8]]

tbl1 = PptxTable(data1)
tbl1.create_table()
tbl1.save_pptx("test1.pptx")
```

![](/docs/test1.png)


#### Set location of table and provide some formatting
-------------------------------------------------

```
from pptx.util import Inches, Pt  # this comes from Python-pptx

tbl2 = PptxTable(data1)
tbl2.set_table_location(left=Inches(0), top=Inches(3), width=Inches(5))
tbl2.set_formatting(font_size=Pt(7), row_height=Inches(.5))
tbl2.create_table(slide_index=0)
tbl2.save_pptx("test2.pptx")
```
![](/docs/test2.png)
#### Create column headers
---------------------

```
tbl3 = PptxTable(data1)
tbl3.set_table_location(left=Inches(0), top=Inches(3), width=Inches(5))
tbl3.set_formatting(font_size=Pt(7), row_height=Inches(.5))
tbl3.create_table(slide_index=0,
                  columns_headers=["column0", "column1", "column2"])
tbl3.save_pptx("test3.pptx")
```
![](/docs/test3.png)


#### Sort columns
------------

```
tbl4 = PptxTable(data1)
tbl4.set_table_location(left=Inches(0), top=Inches(3), width=Inches(5))
tbl4.set_formatting(font_size=Pt(9), row_height=Inches(.5))
tbl4.create_table(slide_index=0,
                 columns_sort_order=[2, 1, 0],
                 # notice the column headers need to be changed to match the column sort order
                 columns_headers=["column2", "column0", "column1"])
tbl4.save_pptx("test4.pptx")
```

![](/docs/test4.png)


#### Set column widths
-----------------

```
tbl5 = PptxTable(data1)
tbl5.set_table_location(left=Inches(0), top=Inches(3), width=Inches(5))
tbl5.set_formatting(font_size=Pt(9), row_height=Inches(.5))
tbl5.create_table(slide_index=0,
                  columns_sort_order=[2, 1, 0],
                  # notice the column headers need to be changed to match the column sort order
                  columns_headers=["column2", "column0", "column1"],
                  # the numbers in the list correspond to the weight given to each column, 1 means unchanged
                  columns_widths_weight=[.75, .75, 1.5])
tbl5.save_pptx("test5.pptx")
```
![](/docs/test5.png)



#### Add another table to the same slide
-----------------------------------

```
here is some new data, oh by the way, it's also formatted differently
data2 = [{"apples": 0, "bananas": 1, "pears": 2},
         {"apples": 3, "bananas": 4, "pears": 5},
         {"apples": 6, "bananas": 7, "pears": 8}]

# get the presentation containing the previous table
presentation = tbl5.prs
tbl6 = PptxTable(data2, presentation)
tbl6.set_table_location(left=Inches(0), top=Inches(5), width=Inches(4))
tbl6.create_table(slide_index=0,
                   # default sort order is alphabetically on the keys,
                  # so the column headers should be alphabetical in this case
                  columns_headers=["Apples", "Bananas", "Pears"])
tbl6.save_pptx("test6.pptx")
```
![](/docs/test6.png)


#### Transpose a table
-----------------

```
tbl7 = PptxTable(data2)
tbl7.set_table_location(left=Inches(2), top=Inches(1), width=Inches(4))
tbl7.create_table(slide_index=0,
                  columns_headers=["Apples", "Bananas", "Pears"],  # column headers become the row headers
                  columns_widths_weight=[1.5, .5, .5, .5],  # since transpose need 4 columns weights instead of 3
                  transpose=True)
tbl7.save_pptx(os.path.join(here, "docs", "test7.pptx"))
```
![](/docs/test7.png)


#### Pptx-tables multipage
-----------------
```
from pptx import Presentation
from pptx.util import Inches, Pt
from pptx_tables import PptxTable

prs=Presentation()
prs.slide_width = Inches(13.333)
prs.slide_height = Inches(7.5)


data = [
        [['Aa', 'Bb','Cc'],
        [3,     4,      5],
        [6,     7,      8]],

        [['Dd', 'Ee','Ff'],
        [0,     1,      2],
        [6,     7,      8]]
        ]



for n,lst in enumerate(data):

    lyt=prs.slide_layouts[6] # choosing a slide layout
    slide=prs.slides.add_slide(lyt) # adding a slide
    # title=slide.shapes.title 
    title_name = f"Page : "+str(n)
    # title.text=title_name
    # subtitle=slide.placeholders[1]


    tbl = PptxTable(lst,prs)
    tbl.set_table_location(left=Inches(1.5), top=Inches(2), width=Inches(5))
    # tbl.set_formatting(font_size=Pt(7), row_height=Inches(.3),alignment=PP_PARAGRAPH_ALIGNMENT.LEFT)
    tbl.set_formatting(font_size=Pt(12), row_height=Inches(.85))        
    tbl.create_table(slide_index=n,
                      columns_widths_weight=[2, 2, 2],
                      transpose=True,
                      )


tbl.save_pptx("slide_table.pptx")
```
![](/docs/test8.png)
