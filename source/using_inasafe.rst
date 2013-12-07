=======================================
Using InaSAFE
=======================================

InaSAFE Options
...............

The InaSAFE plugin provides an options dialog which allows you to define
various options relating to how InaSAFE will behave. 

1. The options dialog can be
launched by clicking on the InaSAFE plugin toolbars options icon (as shown
below) or from QGIS :menuselection:`Plugins --> InaSAFE --> InaSAFE Options`.

.. image:: images/inasafe/inasafe_plugin_option_icon.png
   :align: center
   :width: 120 pt
 
2. Then the dialog will appear, looking something like 
this:

.. image:: images/inasafe/inasafe_plugin_option.png
   :align: center
   :width: 300 pt

.. note:: You can click on the :guilabel:`Help` button at any time and it will open the
   help documentation browser to this page. 

The following options are available on the Options Dialog:

* **Only show visible layers in the InaSAFE dock:** This option will determine
  whether (when unchecked) all hazard, exposure and impact layers should be
  listed in the InaSAFE dock combo boxes; or (when checked) only visible
  layers.
* **Set QGIS layer name from title in keywords:** This option will (when
  enabled) cause QGIS to name layers in the Layers tree, using the title
  keyword in the layers keywords file. If the layer has no title in its
  keywords, or it has no keywords at all, the normal QGIS behavior for naming
  layers will apply.
* **Zoom to impact layer on scenario estimate completion:** This option will
  cause the map view to zoom in/out in order to completely contain the InaSAFE
  impact scenario map output when an analysis is completed.
* **Hide exposure layer on scenario estimate completion:** This option will
  cause QGIS to turn off the exposure layer used when InaSAFE completes the
  current analysis. You can re-enable the layer visibility again by checking
  its checkbox in the legend.
* **Keyword cache for remote datasources:** This option is used to determine
  where keywords are stored for datasets where it is not possible to write them
  into a .keywords file. See Keywords System for more information on the
  keywords system.
* **Run analysis in separate thread (experimental):** This option cause the
  analysis to be run in its own thread.

.. warning::
   * It is not recommended to use the threaded implementation at this time. For
     this reason it is disabled by default.
   * Pressing Cancel at any time will close the options dialog and any changes
     made will not be applied.
   * Pressing OK at any time will close the options dialog and any changes made
     will be applied immediately.
   * The exact button order shown on this dialog may differ depending on your
     operating system or desktop environment.

Adjust Projection
.................

Before continuing we need to turn one more QGIS functionality on, to enable all
data layers display in one projection (`WGS-84`).

1. For that, go to QGIS 
On the lower right, `Click` |crs| :guilabel:`CRS status`.

2. Tick the :guilabel:`Enable on  the fly CRS transformation` box. And then :guilabel:`OK`.

.. image:: images/inasafe/inasafe_crs.png
   :align: center
   :width: 300 pt

Now, any data layer that we will integrate into our project will be adjusted on
the same coordinate.

Exploring InaSAFE Plugin
........................

1. You can drag and drop the dock panel to reposition it in the user interface.
For example, dragging the panel towards the left margin of the QGIS
application will dock it to the left side of the screen.

2. Depending on your preference you could show the :guilabel:`Layer` and :guilabel:`InaSAFE` 
panel at the same time.

Or have the :guilabel:`Layer` and :guilabel:`InaSAFE` panels in a tab systems.

Or for more convenience, having them on top of each other.

.. image:: images/inasafe/inasafe_panel_above_layer.png
   :align: center
   :width: 300 pt

The INASAFE panel contains 3 sections: **Questions, Results** and **Buttons.**
We will explore those sections one by one.

The Questions Section
.....................

The intention of InaSAFE is to make it really simple and easy to perform your
impact analysis. The Questions area provides a simple way for you to formulate
what it is you want to find out? All questions are formulated in the form:

*In the event of* **[hazard]** *how many* **[exposure]** *might* **[impact].**

For example:
In the event of a **flood** how many **buildings** might be **closed**?

In order to answer such question, InaSAFE developers have built a number of
impact functions that cover risk scenarios such as flood, tsunami, volcanic ash
fall, earthquake and so on. In our case, we will use the flood impact function.

To answer our question **In the event of a flood, how many buildings might be
closed**, we need to complete all the areas in the Questions section: hazard,
exposure, impact.

Hazard
......

Hazard is the physical event that creates the risk.

A hazard (in **the event of**) may be represented as a raster layer or as an
area (polygon). For example:

* **Raster:** where each pixel in the raster represents the current flood depth
  following an inundation event.
* **Polygon:** where it has been identified that flood has existed in that area
  (this will not have depth related information)

For our exercise, we will use Quiapo data. Those data
are on your computer at :file:`~/quiapo/`. 

1. We will add the hazard layer in the INASAFE dock. For that, we need to add
the hazard layer from QGIS first. The flood layer is in a raster format, so we
will go to the QGIS menu, click on :menuselection:`Layer --> Add Raster Layer`.

.. note::
   InaSAFE 2.0 is now allowing to import hazard layer in a vector format. 

2. Once you click on that, a pop-up window will appear where you will have to
fetch your flood data.  Please select the
:file:`flood_5yr.tif` file from the :file:`~/quiapo/raster/flood` directory.

.. image:: images/inasafe/quiapo_5yr_flood.png
   :align: center
   :width: 300 pt

This is a raster data (in GTiff format) that represents flooding depth in 
Quiapo area for a return period of 5 years. Adding style on raster.
Right click on the `flood_5yr` the select `Properties`. On `Style` tab,
click `Load Style ...` Use the `flood.qml` as your 
raster style.  file from the :file:`~/quiapo/raster/flood`.

.. image:: images/inasafe/load_style.png
   :align: center
   :width: 300 pt

.. image:: images/inasafe/quiapo_5yr_flood_style.png
   :align: center
   :width: 300 pt

You will notice that the layer filled automatically the :guilabel:`hazard` area in the
InaSAFE dock panel. There are two important things to note when uploading
data in InaSAFE.

* Data should follow a keyword metadata system that allows InaSAFE to determine
  if the layer is a hazard or if it is an exposure.
* The area of analysis should overlap.

Adding keyword metadata
.......................

You may be wondering how the InaSAFE plugin determines whether a layer should
be listed in the :guilabel:`In the event of` :guilabel:`How many` combo boxes? The plugin 
relies on simple keyword metadata to be associated with each layer. Each layer
that has a keyword allocating it's category to hazard will be listed in the 
:guilabel:`In the event of` combo. Similarly, a category of exposure in the keywords for a
layer will result in it being listed under the :guilabel:`How many` combo. InaSAFE uses
the combination of category, subcategory, units and datatype keywords to
determine which impact functions will be listed in the :guilabel:`Might` combo.

In our exercise, the keywords were already created, so the data could fill
automatically the :guilabel:`In the event of` :guilabel:`How many combo` boxes. If the keywords
were not created in advance, then we will create them by following one of the
two steps:

1. Go to the InaSAFE tools on the toolbar, click on the :guilabel:`Keyword Editor` 
icon.

.. image:: images/inasafe/inasafe_keyword.png
   :align: center
   :width: 150 pt

Or, open the :menuselection:`Plugin` menu on QGIS toolbar, click on 
:menuselection:`InaSAFE`, then click
on the :menuselection:`Keyword Editor` in the scroll list.

.. image:: images/inasafe/inasafe_keyword_editor_menu.png
   :align: center
   :width: 300 pt

2. Once you click on the :guilabel:`Keyword Editor`, a dialog box containing the flood data
will be prompted. Since the flood data is a hazard layer, pinpoint the
:guilabel:`Hazard` Category. In the Subcategory, we will choose :guilabel:`flood [m]` 
because our data represents depth of flood in Manila in meter unit.

.. image:: images/inasafe/inasafe_hazard_keyword.png
   :align: center
   :width: 300 pt

3. Then click 
:guilabel:`OK`.

Now the data follow the keyword rule, and can be used in the InaSAFE function.

Exposure
........

Exposure is the sum of assets and population that are at risks.

An exposure (How many) layer could be represented, for example, as vector
polygon data representing building outlines, or a raster outline where each
pixel represents the number of people resident in that cell.

Now, we will add the exposure layer in our InaSAFE project. For that, we need
to add the exposure layer to QGIS first. For our exercise, we will use the
data that represents buildings.

1. The OSM building layer is in a vector format, so we will go to the QGIS menu
toolbar, click on :menuselection:`Layer --> Add Vector Layer`.

Please note that the exposure data should follow the same keyword system
that we explained earlier for the hazard data.

We will create it by using the :guilabel:`Keyword Editor`.

2. Go to the :menuselection:`Plugin --> InaSAFE --> Keyword Editor` in the dialog box. 
Pinpoint the :guilabel:`Exposure` category.

3. Choose :guilabel:`structure` in the :guilabel:`Subcategory` scroll box. Click 
:guilabel:`OK`.

.. image:: images/inasafe/inasafe_exposure_keyword.png
   :align: center
   :width: 300 pt

Now our OSM building exposure data can be used in INASAFE and was automatically
entered in the :guilabel:`How many` box of the INASAFE dock panel.

.. image:: images/inasafe/inasafe_hazard_exposure_layers.png
   :align: center
   :width: 300 pt

Impact Function Configuration
...............

This configuration has a 2 to 3 tabs which are `Options, Postprocessors and Minimum Needs`
which can be customize for the impact result. 

On the `Option` tab, You can see the `Threshold`
plus the unit of measurement where we're going to base who are affected and not affected.
For example in flood, threshold are in meter so when we added 1 in the textbox it means Buildings
or People who are in 1 meter and above are the affected.

.. image:: images/inasafe/threshold1.png
   :align: center
   :width: 150 pt

On the `Postprocessors` tab, the collected data are being categorized in the impact result.
Since we're using buildings as an exposure, it will categorized who are flooded and not flooded
according to building types.

.. image:: images/inasafe/postprocessors1.png
   :align: center
   :width: 150 pt

Postprocessor for the people who needs to be evacuated has a different categories. There are
`Gender, Age and Minimum Needs` It is categorized in gender to recognized women which 
will received hygiene packs and for the lactating women who whill received additional rice pack. 
Age are being categorized especially for the elder to have a better evacuation center.

.. image:: images/inasafe/postprocessors2.png
   :align: center
   :width: 150 pt

The `Minimum Needs` tab purpose is to give estimated relief packs to be given in every family
who are affected by flood.

.. image:: images/inasafe/min_needs.png
   :align: center
   :width: 150 pt


Impact Analysis
...............

The impact function (:guilabel:`Might`) will spatially combine the hazard and exposure
input layers in order to postulate what the impacts of the hazard will be on
the exposure infrastructure or people. By selecting a combination from the 
:guilabel:`In the event of` and :guilabel:`How many` combo boxes, an appropriate set 
of impact functions will be listed in the :guilabel:`Might` combo box.

Impact scenarios are predefined depending on what the decision-maker is looking
for. For our flood analysis in Quiapo, we only have on predefined impact
function which asks: **In case of flood event, how many buildings might be
temporarily closed?** As we see on the previous step, this is filled
automatically by default in the InaSAFE panel dock as soon as the hazard
[flood] and exposure [buildings] layers are entered correctly.

The Results section
...................

1. Now that we have our two input layers and that we know what impacts we would
like to assess, click on the :guilabel:`Run` button at the bottom to start the impact
analysis. At the end of the process, figures will be shown in the 
:guilabel:`Results` section, a new layer will be added in the QGIS layer panel representing the
result of the impact function, and the map will differentiate affected and
non-affected building.

.. image:: images/inasafe/inasafe_flood_impact_results.png
   :align: center
   :width: 300 pt

2. The result shows **total number of buildings** and the 
**number of buildings that might be temporarily closed** in the event of a flood. 
Also, there is an **Action Checklist** where the question: 
*Are the critical facilities still open?* And a **Note** description explaining 
that buildings are said flooded when the flood level exceeds 1 meter.
 
Print Results
.............

The data shown on the screen can be saved into a **PDF file** by clicking on
:guilabel:`Print` at the bottom of the InaSAFE panel and a message box will appear.
On `Area to print` then click `Open PDF` button. The PDF file contains then the
legend for the result of the impact assessment, the map created and a
table summarizing the results from the impact function.

However, any change that you want to make into the final map document should be
done before clicking on the :guilabel:`Print` button of the InaSAFE dock panel. The
print should be only use once the data is exactly as you want it to be
displayed.

.. image:: images/inasafe/inasafe_pdf_output.png
   :align: center
   :width: 500 pt

Save results and QGIS project
.............................

1. The output layer result of the assessment can be saved by right clicking on the
layer.

.. image:: images/inasafe/inasafe_save_as_vector1.png
   :align: center
   :width: 300 pt

2. Then :guilabel:`Save As` a shapefile or a raster. A new message box will appear,
click `Browse` button then choose what directory to be used for saving. However the
keywords and statistics do not get saved.

.. image:: images/inasafe/save_as.png
   :align: center
   :width: 300 pt

.. image:: images/inasafe/inasafe_save_as_vector2.png
   :align: center
   :width: 300 pt

3. You can also save the project under QGIS so that you can access your current
window view anytime needed. 

Now that the project is saved under QGIS, you can go back to your work anytime
you need. However, the statistical data will be lost whenever the project is
closed. To get the data back, you will need to redo the impact analysis process
we described above from :guilabel:`Run`.

Further exercise
------------------

Using the data in your Quiapo directory answer the following questions with
Inasafe:

* In case of **flood (100 year)** event, how many **buildings might be temporarily closed**?
* In case of **flood (100 year)** event, how many **people might need evacuation**?

Explore the other features of InaSAFE.

.. raw:: latex
   
   \pagebreak[4]
