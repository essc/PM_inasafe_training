Volcano Polygon Hazard Population
=================================

Overview
--------

**Unique Identifier**: 
Volcano Polygon Hazard Population

**Author**: 
AIFDR

**Rating**: 
4

**Title**: 
Need evacuation

**Synopsis**: 
To assess the impacts of volcano eruption on population.

**Actions**: 
Provide details about how many population would likely be affected by each hazard zones.

**Hazard Input**: 
A hazard vector layer can be polygon or point. If polygon, it must have "KRB" attribute and the valuefor it are "Kawasan Rawan Bencana I", "Kawasan Rawan Bencana II", or "Kawasan Rawan Bencana III."If you want to see the name of the volcano in the result, you need to add "NAME" attribute for point data or "GUNUNG" attribute for polygon data.

**Exposure Input**: 
An exposure raster layer where each cell represent population count.

**Output**: 
Vector layer contains population affected and the minimum needs based on the population affected.

Details
-------

No documentation found

Docstring
----------

Impact function for volcano hazard zones impact on population

    :author AIFDR
    :rating 4
    :param requires category=='hazard' and                     subcategory in ['volcano'] and                     layertype=='vector'

    :param requires category=='exposure' and                     subcategory=='population' and                     layertype=='raster'
    