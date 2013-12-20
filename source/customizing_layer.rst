===============================================
Pre-processing layers for use InaSAFE
===============================================

Before using your data layers (hazard and exposure) in InaSAFE certain 
preprocessing are needed such as attribute and format conversions.
This will ensure that the layers and its values are correct in order to
provide a reliable result. 

Computing area and perimeters of a polygon layer
-------------------------------------------------------------------

1. :menuselection:`Plugins --> Manage and install plugins...`

2. Type `fTools` on the search bar then enable.

3. Load your layer .

4. :menuselection: `Vector --> Geometry Tools --> Export/Add geometry columns`

5. Choose where layer will the Area and Perimeter data be added. Choose `Layer CRS` for calculation.

   .. image:: images/inasafe/brgy_area.png
      :align: center
      :width: 300 pt

.. Note::
   Make sure that your layer has a correct CRS before choosing `Layer CRS` for calculation.

6. Check if Area and Perimeter were added, Right click on the layer then `Open Attribute Table`

Now that we have a population and area on our data we're going to add a new column for population density.

1. On `Open Attribute Table` click |mActionToggleEditing| then |mActionCalculateField| .

2. On `Output field name` Add name for population density (e.g., popden).

   .. image:: images/inasafe/fld_calc.png
      :align: center
      :width: 300 pt

3. Add this formula on 'Expression' `Population / (Area/10000)`

To do that:

* On the `Function List` choose `Fields and Values`.
* First, double click population column which is '2010_Pop'.
* Add division sign '/'.
* Click open parenthesis, double click 'AREA' in `Fields and Values ` divided by 10000 then close parenthesis.
* Click, `OK`, Save |mActionFileSave| and click |mActionToggleEditing|.

Adding Numerical Data for Hazards
-------------------------------------

1. Right click your *Flood* layer, then select `Open Attribute Table`.

2. Click |mActionToggleEditing|.

3. Sort your data by clicking the field name:

   .. image:: images/inasafe/sort_data.png
      :align: center
      :width: 300 pt

4. Selecting by group of data by clicking on the row number `Shift` (Keyboard) up to the last data.

   .. image:: images/inasafe/haz_calc.png
      :align: center
      :width: 300 pt

5. Click |mActionCalculateField| .

6. Since we're going to make a new field, check `Create a new field`.

7. On `Output field name` Add name for hazard level (e.g., haz_level).

   .. image:: images/inasafe/haz_calc.png
      :align: center
      :width: 300 pt

8. Set `Expression` according to your flood data.

code::
 Low = '1'
 Medium = '2'
 High = '3'

9. Now that we already have a field for numerical hazard data, on `Field Calculator` check `Update existing field`.

   .. image:: images/inasafe/exist_calc.png
      :align: center
      :width: 300 pt

10. Click, `OK`, Save |mActionFileSave| and click |mActionToggleEditing|.

Converting Vector to Raster
--------------------------------------

**Population Density**

1. :menuselection:`Raster --> Conversion --> Rasterize`.

2. On `Input file (shapefile) select vector layer to be rasterize.

3. On `Attribute field` field name of *Population Density* to be rasterize.

   .. image:: images/inasafe/input_attr.png
      :align: center
      :width: 300 pt

4. On `Output file for rasterized vecto(raster)` click *Select* button.

5. Choose file path for saving the raster, add name select `.TIF` as image format then *Save*.

   .. image:: images/inasafe/pop_tif.png
      :align: center
      :width: 300 pt

6. Click `Raster resolution in map units per pixel`.
Set the `Horizontal` and `Vertical` to 100.

7. Click, *OK* then choose `WGS 84 / UTM zone 51N` as your projection.

8. Click `OK` twice then `Close`.

.. Warning::
   Look out for the OK button you're clicking you may duplicate creating raster layer.

**Flood**

Same instruction for Population Density. Just change layer name and attribute field like this:

   .. image:: images/inasafe/haz_attr.png
      :align: center
      :width: 300 pt

Re-projecting raster layers
----------------------------

We're going to set your projection to WGS 84 because it is the default projection which InaSAFE can recognize.

1. Right click on raster layer.

2. Select `Save as..`.

3. `Browse` where to save raster layer. Then save as `.tif` file.

   .. image:: images/inasafe/saveas_wgs84.png
      :align: center
      :width: 300 pt

4. `Change` then Choose `WGS 84` as CRS.

   .. image:: images/inasafe/wgs84.png
      :align: center
      :width: 300 pt

5. Click `OK`.

Setting the the layers in the Keyword Editor
-----------------------------------------------

**Hazard**

1. Click on our hazard layer with WGS 84 projection (e.g, haz_level_wgs84)

2. Click Keyword Editor |keyword_editor|

3. Select `Hazard`

4. Click `Show advanced editor`

Keyword: *subcategory* Value: *flood*
Keyword: *unit* Value: *m*

5. `Add to list` then `OK`

   .. image:: images/inasafe/key_haz.png
      :align: center
      :width: 300 pt

.. Note::
   We're just trying to demo how your data will work on InaSAFE.
   1m, 2m, 3m are not representing Low, Moderate and High. Better
   if you input your flood depth in meters at Attribute Table. 
   Categorical hazard data are still on development. We will 
   update you once it is done.

**Population**

1. Click on our exposure layer with WGS 84 projection (e.g, popden_wgs84)

2. Click Keyword Editor |keyword_editor|

3. Select `Exposure`

4. Click `Show advanced editor`

Keyword: *subcategory* Value: *population*

5. `Add to list` then `OK`

   .. image:: images/inasafe/key_pop.png
      :align: center
      :width: 300 pt

**Aggregation**

1. Select your `Administrative Boundary` layer which is vector

2. Click Keyword Editor |keyword_editor|

3. Select `Postprocessing`

4. Set `Aggregation attribute` as Barangay

   .. image:: images/inasafe/aggr.png
      :align: center
      :width: 300 pt

5. Click `OK`

Click `Run`

.. raw:: latex
   
   \pagebreak[4]
