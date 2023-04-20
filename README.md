# CellDiversityRao (v1)
<a name="readme-top"></a>

<!-- PROJECT SHIELDS -->
<!-- https://github.com/Ileriayo/markdown-badges -->
[![R version](https://img.shields.io/badge/R%3D-4.2.3-6666ff.svg)](https://cran.r-project.org/)

<!-- ABOUT THE PROJECT -->
## About The Project

The application of Rao diversity metric to measure the environmental heterogeneity of specific immune cells.

Main aspects of CellDiversityRaoce:
* (describe here how the json files are generated).
* Use of subtype of immune cells to extract spatial diversity features (use of Rao metric - [Spectral Rao](https://github.com/mattmar/spectralrao)).
* Use of raster to depict the spatial patterns of the diversity features.
* Generate single feature descriptor per file to be used for further analysis.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

List of major frameworks/libraries used to build the project.

* [![RStudio](https://img.shields.io/badge/RStudio-4285F4?style=for-the-badge&logo=rstudio&logoColor=white)](https://posit.co/downloads/)
* [![R](https://img.shields.io/badge/r-%23276DC3.svg?style=for-the-badge&logo=r&logoColor=white)](https://cran.r-project.org/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

Cell Diversity Rao consists of three segments. 

1. The preparation of the json file data.
2. The feature extraction of the Rao features.
3. The plot or depiction in 2D of the spatial Rao features.

### Data preparation
1. We unzip a testing json file located in the /data/ folder. The file is testing_image.zip. Inside the file that is used for testing the scripts is testing_image.json.
2. The file has information related to the position of different type of cells, in this case we identified necrotic, neoplastic, inflammatory, connective and non-neoplastic epithelial cells. Also other type of position of cells can be used (e.g. subtype of immune cells such as CD4+, CD8+ and CD4+) as long as they are saved in the json format. Minor adjustments are neded to be saved as a csv for processing.

3. The cell position and identification (x and y position and label 'cell type') are taken to be converted as raster (rasterize), a graphical representation of 2D. The spectralrao function is used to do so. Previously, a empty raster is created with specific number of rows and columns (matrix) so the same canvas is used for every type of cell or representation, as it is needed to be compared across, to obtain a true diversity measurement.
4. The spectralrao function generates a rao matrix. In this example, we use the option of 'multidimension' and euclidean distance. The multi-dimension stands for taking into account different type of popultions of cells, calculating the Rao index across the diversity of cells. From this rao matrix, we can either plot it, save it, calculate statistical moments (e.g. mean or median of the matrix, density distribution).
5. To plot the example, we use the spplot function. First, the rao matrix is re-rasterize and rescale (0,1). The dimenions are the same as the canvas, if more details is needed, the canvas size can be increased.

### Rao Feature extraction
The rao matrix can is adjusted to be a vector and transposed. Any NaN values found are removed. Then the statistical moments can be calculated. This values are then representing each image or file or case.
```R
   # we transform the rao matrix as vector and transpose it.
raomatrix2_trans <- as.vector(t(raomatrix2[[1]]))
# we remove any nan values or are not taken into account
raomatrix2_trans <- raomatrix2_trans[!is.na(raomatrix2_trans)]
# sort the values for further processing
raomatrix2_trans <- sort(raomatrix2_trans, decreasing = TRUE)
# Calculate the basics statistics from the matrix
# mean, median, max, min or standard deviation
mean(raomatrix2_trans)
   ```
### Plot of the Rao features and representation
 To plot, simply use the spplot function or plot directly with the raster function raster::plot(raster(rao matrix)).
   ```R
rao_mat1 <- c(r1,r2,r3,r4,r5)
raomatrix1<-spectralrao(input=rao_mat1,window=3,mode="multidimension",distance_m="euclidean",lambda=0.5,na.tolerance=0.85,rescale=FALSE, shannon=F)
spplot(rescale0to1(rast(raomatrix1[[1]])), col.regions = c(topo.colors(50),rev(heat.colors(50))), at=seq(0,1, by=0.01), xlab = "Rao (x-axis)", ylab = "Rao (y-axis)",main=list(label="All cells",cex=2))
   ```
   
<!-- USAGE EXAMPLES -->
## Usage

Some screenshots and image generated can be observe below.

<!-- _For more examples, please refer to the [Documentation](https://example.com)_ -->

### R output examples

The example plots generated by R. (left) The plot of the raster::plot and (right) the combination of multiple raster plots of the different cell types. Below is the example of using a raster canvas of higher value, of which the detail of the values can be seen.

<p align="center">
  <img alt="Light" src="https://github.com/maberyick/CellDiversityRao/blob/main/images/raster_Rao_plot.png" width="35%">
&nbsp; &nbsp; &nbsp; &nbsp;
  <img alt="Dark" src="https://github.com/maberyick/CellDiversityRao/blob/main/images/Rplot0.png" width="54%">
&nbsp; &nbsp; &nbsp; &nbsp;
  <img alt="Dark" src="https://github.com/maberyick/CellDiversityRao/blob/main/images/Rplot2.png" width="70%">
&nbsp; &nbsp; &nbsp; &nbsp;
   Image with more detail by increasing the raster canvas size.
  <img alt="Dark" src="https://github.com/maberyick/CellDiversityRao/blob/main/images/Rplot1.png" width="70%">
</p>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap

- [x] Update the README
- [x] Add back running scrips
- [ ] Add Additional Templates w/ Examples
- [ ] Add "components" document to easily copy & paste sections of the readme
- [ ] Multi-language Support
    - [ ] Spanish

Future uses will be added once are found or observed.

<!-- See the [open issues](https://github.com/othneildrew/Best-README-Template/issues) for a full list of proposed features (and known issues). -->

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Reference
Please cite the paper below if you want to implement this approach:

Lopez de Rodas M, Nagineni V, Ravi A, Datar IJ, Mino-Kenudson M, Corredor G, Barrera C, Behlman L, Rimm DL, Herbst RS, Madabhushi A, Riess JW, Velcheti V, Hellmann MD, Gainor J, Schalper KA. Role of tumor infiltrating lymphocytes and spatial immune heterogeneity in sensitivity to PD-1 axis blockers in non-small cell lung cancer. J Immunother Cancer. 2022 Jun;10(6):e004440. doi: 10.1136/jitc-2021-004440. PMID: 35649657; PMCID: PMC9161072.

  ```
Lopez de Rodas M, Nagineni V, Ravi A, Datar IJ, Mino-Kenudson M, Corredor G, Barrera C, Behlman L, Rimm DL, Herbst RS, Madabhushi A, Riess JW, Velcheti V, Hellmann MD, Gainor J, Schalper KA. Role of tumor infiltrating lymphocytes and spatial immune heterogeneity in sensitivity to PD-1 axis blockers in non-small cell lung cancer. J Immunother Cancer. 2022 Jun;10(6):e004440. doi: 10.1136/jitc-2021-004440. PMID: 35649657; PMCID: PMC9161072.
   ```
   
<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

Cristian Barrera - cbarrera31@gatech.edu

Project Link: [https://github.com/maberyick/CellDiversityRao](https://github.com/maberyick/CellDiversityRao)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
