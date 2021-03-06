# 2021-0294105_NH3_EFs
Calculation of selected emission factors for NH3 loss from field-applied manure

# Overview of calculations
These calculations follow a recent report (Hafner et al., 2021), with some small changes in model inputs.
The report is available from <https://pure.au.dk/portal/files/223538048/EFreport23092021.pdf> and the calculations are described in detail at <https://github.com/sashahafner/ALFAM2-EF-DK-2021>.

Briefly, the ALFAM2 model (v1.5.1 of the ALFAM2 R package, Hafner et al., 2020) was used with Parameter Set 2 to calculate cumulative NH3 emission after 168 hours.
Model inputs included manure dry matter (DM) and pH, average air temperature and wind speed, and application method.

# Weather inputs
Weather inputs for model calculations are adjusted means for the decade 2010-2019.
These means are calculated in the `../weather` directory, which includes a `readme.md` file explaining data sources and calculations in some detail.
Unlike the earlier work (Hafner et al., 2021), emission factors calculated here are based on monthly averages for rainfall.
There are small differences in air temperature and wind speed as well--see `../weather/readme.md` for details.
And here, summer was defined as June, July, and August for all emission factors.

# Manure dry matter and untreated pH
For cattle and pig manure (kvæg and svin in Danish), these inputs exactly match those given in the earlier work (Hafner et al., 2021).
For digestate (afgasset in Danish), an updated dry matter (DM) value was used, calculated as the mean of 26 samples consisting of the combination of the 15 samples from Møller and Nielsen (2016) as described in Hafner et al. (2021) and 11 samples recently analyzed at Aarhus University (these 11 are the same samples from Nyord et al. (2021) but with some additonal samples collected later).
The mean DM value is calculated in the `../manure_composition` directory (see `scripts_digestate` and `output`).
The mean pH for these 26 samples was identical to the value used in Hafner et al. (2021).

# Acidification emission factor calculations
To account for acidification, the ALFAM2 model predicts the effect of manure pH on emission.
The larger report (Hafner et al., 2021) used a fixed pH value of 6.4 for field acidification.
In the current regulation, acidification extent is described by a required acid dose, and not a fixed pH value.
Required acid doses are given in the table below.
To translate these doses into a fixed pH value for each manure type, a change in pH ($delta$pH) due to acidification was applied to average un-acidified manure pH for Denmark (7.0 for cattle, 7.2 for pig, and 7.9 for digestate from Hafner et al., 2021).
The $delta$pH value for the acid doses given above was determined using the titration measurements described in Nyord et al. (2021).
As described in the earlier report (Nyord et al., 2021), four types of manure samples were titrated with sulfuric acid.
In the present analysis $delta$pH (relative the initial pH value) resulting from the acid doses given above was determined by interpolation.
Mean $delta$pH values were calculated for all available samples: 11 afgasset, 8 kvæg, 7 slagtesvin, and 4 so-/smågrise samples.
Results from each sample were based on 2 measured titration curves from separate sub-samples.
For pig, an average value for the two types of manure was used here.
Results are given below.

| Type of manure | Dose (kg/t)| Un-acidified pH | Ave. pH change | Acidified pH |
|----------------|------------|-----------------|----------------|--------------|
|       Kvæg     |      3.0   |      7.0        |    -0.53       |   6.47       |
|       Svin     |      2.9   |      7.2        |    -0.73       |   6.47       |
|     Afgasset   |     11.0   |      7.9        |    -1.38       |   6.52       |
Notes: Acid doses are from <https://www.retsinformation.dk/eli/lta/2021/1551>.

It is a coincidence that acidified pH values are identical for cattle and pig manure.

For details on the titration curves and how the pH values in the table above were calculated, see the `../manure_titration` directory.
The $delta$pH values used here were taken from `../manure_titration/output/dpH_acid_doses.csv` file.

Although the dose and resulting manure pH values are linked in the analysis in `../manure_acidification/acid_preds` and Nyord et al. (2021), with non-linear responses, means are not necessarily identical.

# Repeating calculations
Calculations can be repeated in R by running the script `scripts/main.R`.
The `logs` directory has a log of the ALFAM2 model call.
Inputs are specified in the `inputs` directory.

# References
Hafner, S. D., Nyord, T., Sommer, S. G., & Adamsen, A. P. S. 2021. Estimation of Danish emission factors for ammonia from field-applied liquid manure for 1980 to 2019.138 pages. Advisory report from DCA – Danish Centre for Food and Agriculture, Aarhus University, submitted: 23-09-2021. <https://pure.au.dk/portal/files/223538048/EFreport23092021.pdf>

Møller, H.B., Nielsen, K.J., 2016. Biogas taskforce – udvikling og effektivisering af biogasproduktionen i Danmark.
DCA report 077. DCA - Nationalt Center for Fødevarer og Jordbrug, Tjele, Denmark.

Nyord, T., Hafner, S.D., Adamsen, A.P.S., Sommer, S.G. 2021. Ammoniakfordampning fra forsuret gylle ved udbringning med slæbeslange. DCA - Nationalt Center for Fødevarer og Jordbrug Aarhus Universitet. Journal 2020-0188079. <https://pure.au.dk/portal/files/211563336/Forsuring_150221.pdf>
