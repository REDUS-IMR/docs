# REDUS Master Recipe

REDUS framework uses a special kind of recipe to process the IMR Survey Time Series from NMD's Dataset Explorer. This recipe is formulated in an XML format and contains many user configurable options. 

## Examples

Below are some recipe samples:

### 1. Single Survey Time series

```xml
<?xml version="1.0" encoding="UTF-8"?>
<redus_master xmlns="http://www.imr.no/formats/redus/master/v0.1" revision="1" version="0.1">
  <configuration>
    <saveOutputTable>true</saveOutputTable>
    <saveRunStatus>false</saveRunStatus>
  </configuration>
  <globalParameter>
    <!-- <fileFix from="biotic_cruiseNumber_1994001_Anny+KrÃ¦mer.xml" to="biotic_cruiseNumber_1994001_Anny+Kræmer.xml"/> -->
  </globalParameter>
  <parameters sts="Barents Sea Northeast Arctic cod bottom trawl index in winter" revision="1" version="0.1">
    <configuration>
      <stsName>Barents Sea Northeast Arctic cod bottom trawl index in winter</stsName>
      <overwriteNMD>false</overwriteNMD>
      <forceReProcess>true</forceReProcess>
      <forceBioticV3>true</forceBioticV3>
      <skipYear>1993</skipYear>
      <startYear></startYear>
      <endYear></endYear>
      <levelRequested>bootstrapImpute</levelRequested>
      <bootstrapSeed>77</bootstrapSeed>
      <bootstrapImputeSeed>101</bootstrapImputeSeed>
      <bootstrapIter>5</bootstrapIter>
      <coresUse>1</coresUse>
      <groupType>age</groupType>
      <minAge>1</minAge>
      <maxAge>12</maxAge>
      <plusAge>7</plusAge>
      <numberScale>1000000</numberScale>
      <dataType>survey</dataType>
      <applyOverrides>false</applyOverrides>
    </configuration>
    <override>
      <!-- <parameter name="UseProcessData">true</parameter> -->
    </override>
  </parameters>
</redus_master>
```

### 2. Multiple survey time series
```xml
<?xml version="1.0" encoding="UTF-8"?>
<redus_master xmlns="http://www.imr.no/formats/redus/master/v0.1" revision="1" version="0.1">
  <configuration>
    <saveOutputTable>true</saveOutputTable>
    <saveRunStatus>false</saveRunStatus>
  </configuration>
  <globalParameter>
    <!-- <fileFix from="biotic_cruiseNumber_1994001_Anny+KrÃ¦mer.xml" to="biotic_cruiseNumber_1994001_Anny+Kræmer.xml"/> -->
  </globalParameter>
  <parameters sts="Barents Sea Northeast Arctic cod bottom trawl index in winter" revision="1" version="0.1">
    <configuration>
      <stsName>Barents Sea Northeast Arctic cod bottom trawl index in winter</stsName>
      <overwriteNMD>false</overwriteNMD>
      <forceReProcess>true</forceReProcess>
      <forceBioticV3>true</forceBioticV3>
      <skipYear>1993</skipYear>
      <startYear></startYear>
      <endYear></endYear>
      <levelRequested>bootstrapImpute</levelRequested>
      <bootstrapSeed>77</bootstrapSeed>
      <bootstrapImputeSeed>101</bootstrapImputeSeed>
      <bootstrapIter>5</bootstrapIter>
      <coresUse>1</coresUse>
      <groupType>age</groupType>
      <minAge>1</minAge>
      <maxAge>12</maxAge>
      <plusAge>7</plusAge>
      <numberScale>1000000</numberScale>
      <dataType>survey</dataType>
      <applyOverrides>false</applyOverrides>
    </configuration>
    <override>
      <!-- <parameter name="UseProcessData">true</parameter> -->
    </override>
  </parameters>
  <parameters sts="Barents Sea capelin acoustic abundance estimate in autumn" revision="1" version="0.1">
    <configuration>
      <stsName>Barents Sea capelin acoustic abundance estimate in autumn</stsName>
      <overwriteNMD>false</overwriteNMD>
      <forceReProcess>true</forceReProcess>
      <forceBioticV3>true</forceBioticV3>
      <skipYear>2016,2019</skipYear>
      <startYear>2016</startYear>
      <endYear>2020</endYear>
      <levelRequested>bootstrapImpute</levelRequested>
      <bootstrapSeed>77</bootstrapSeed>
      <bootstrapImputeSeed>101</bootstrapImputeSeed>
      <bootstrapIter>5</bootstrapIter>
      <coresUse>1</coresUse>
      <groupType>age</groupType>
      <minAge>1</minAge>
      <maxAge>12</maxAge>
      <plusAge>7</plusAge>
      <numberScale>1000000</numberScale>
      <dataType>survey</dataType>
      <applyOverrides>false</applyOverrides>
    </configuration>
    <override>
      <parameter name="UseProcessData">true</parameter>
    </override>
  </parameters>
</redus_master>
```

## Usage

The recipe can be used in a stand-alone processing or included in the REDUS framework workflow. In brief, the processing of the recipe can be done by using the `REDUStools` R package (https://github.com/REDUS-IMR/REDUStools).

```r
install.packages("remotes")
remotes::install_github("REDUS-IMR/REDUStools")
remotes::install_github("SEA2DATA/Rstox", ref="develop")

# Output below can be the processing status or result table
# subject to the 'saveOutputTable' parameter in the XML recipe
output <- REDUStools::processRstoxSTS("barents_sea_neacod_trawl_winter.xml")
```

Please see another guide that will explain more on the above stand-alone process (TODO).

## Parameter details

1. `saveOutputTable`: Setting this node text to `true` tells the script to save the resulting table from the StoX process in an `rds` object on disk. Many of the REDUS framework requires this to be set as `true`. Setting it to `false` tells `REDUStools::processRstoxSTS()` function in the example usage above to return the result table.

2. `saveRunStatus`: Setting this node text to `true` tells the script to save the processing status of the survey time series in an `rds` object on disk.

3. `fileFix`: This is useful if user want to override data input filename(s) from all the survey time series `project.xml` files. It will search for a matching string as in the `from` attribute and replace it to the filename specified in the `to` attribute. This parameter have a global scope, means it will be affecting all the survey time series in the recipe.

4. `parameters`: This node contains the unique identifier of the survey time series, please set a unique name for the `sts` attribute (usually the same name as the survey time series itself). Don't forget to set the `revision` and `version` parameters as well if applicable.

5. `stsName`: This node text indicates the name of the survey time series that will be processed. The names have to be exactly the same as the name found in NMD Dataset Explorer.

6.  `overwriteNMD`: Setting this node text to `true` indicates that you want to overwrite the data that you might have downloaded earlier in your local disk.

7. `forceReProcess`: The script will always re-process the survey time series if set to `true`. Otherwise it will use the result from the previous process.

8. `forceBioticV3`: Some recent biotic data from NMD have been saved with NMD Biotic v3.1 format. Since RstoX can't yet process the data, this switch sometimes necessary to force the header of Biotic v3.1 into Biotic V3. Note that the script won't change the content of the data.

9. `skipYear`: Write the years that you want to skip from the processing and separate them using comma sign if you have more that one. Setting this to empty means that you don't want to skip any years.

10. `startYear`: The start of year that you want to process. Setting this empty will process the earliest year possible.

11. `endYear`: The last year that you want to process. Setting this empty will process the survey time series until the last year possible.

12. `levelRequested`: The StoX output data comes from three possible levels: `SuperIndAbundance`, `bootstrap`, and `bootstrapImpute`. Usually `bootstrapImpute` is the preferred level of output.

13. `bootstrapSeed`: Specify a seed value to be used in the bootstrap process.

14. `bootstrapImputeSeed`: Specify a seed value to be used in the impute process.

15. `bootstrapIter`: Specify the number of iteration for the bootstrap and impute process.

16. `coresUse`: Specify the number of cores that will be utilize for the StoX process.

17. `groupType`: Specify the groupings for the indices. The possible values are `age` and `LenGrp`.

18. `minAge`, `maxAge`, `plusAge`: The script will prepare the output table using these age parameters. This won't affect processing, only the output.

19. `numberScale`: This value will be used as a multiplier for the output table. This won't affect processing, only the output.

20. `dataType`: This is internal identifier for the REDUSframework. Leave it to `survey` for survey time series.

21. `applyOverrides`: If set to `true`, the script will try to override some parameters in the `project.xml` files for all the year in a survey time series. Populate the `<override><parameter>...</parameter></override>` sub-nodes with all overrides that you wish to be applied. For example `<parameter name="UseProcessData">true</parameter>` will search `UseProcessData` nodes in a `project.xml` file and set their values to `true`. You can have multiple overrides for a survey time series.

