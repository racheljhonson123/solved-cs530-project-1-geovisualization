Download Link: https://assignmentchef.com/product/solved-cs530-project-1-geovisualization
<br>
In this first project, you will familiarize yourself with VTK while experimenting with several techniques to visualize 2D scalar data. The topic here is <strong>geovisualization</strong>, in other words the visualization of geographic data. Specifically, you will be visualizing a dataset that combines bathymetry/topography data with satellite imagery of the earth surface. Your visualization will include a graphical user interface that interactively controls certain aspects of the visualization.

Background

<strong>Bathymetry</strong> measures the underwater depth of the ocean floor while topography measures land elevation with respect to sea level. Together, they describe the earth surface’s geometry. For simplicity, I use the term <em>elevation</em> below to jointly refer to both. <u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">NASA’s Blue Marble pro</a></u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">j</a><u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">ect</a></u> offers high resolution elevation datasets along with satellite imagery of the earth surface acquired at different times of the year. In this project you will learn how to use VTK to create compelling interactive visualizations of the elevation data.

Practically, the data consists of grayscale images (<u><a href="http://visibleearth.nasa.gov/view.php?id=73963">relief</a></u> / <u><a href="http://visibleearth.nasa.gov/view.php?id=73934">topo</a></u><a href="http://visibleearth.nasa.gov/view.php?id=73934">g</a><u><a href="http://visibleearth.nasa.gov/view.php?id=73934">raphy</a></u>) and color images (<u><a href="http://visibleearth.nasa.gov/view.php?id=73776">satellite views</a></u><a href="http://visibleearth.nasa.gov/view.php?id=73776">)</a> with resolutions up to 86400 x 43200 (3.7 gigapixels). In each image, the earth surface is sampled at regular angular intervals spanning the domain [−π,π] × [−π/2,π/2] in spherical coordinates (see illustration below).

Specifically, each row of the image corresponds to a parallel (line of constant latitude, shown in red), while each column corresponds to a meridian (line of constant longitude, shown in blue). Note that the North and South poles (where parallels degenerate to points) are each represented by an entire row of uniform value.

Task 1. Height Map Visualization and Texture Mapping

Task summary

Your first task consists in visualizing the elevation dataset through an <strong>interactive height map textured with the satellite image</strong>. See explanations below.

<h2>Height map</h2>

A <strong>height map</strong> is a mapping from a gray-scale (scalar) image to a curved surface, whereby each (x,y) position associated with value f in the image is mapped to the 3D position (x,y,f). In the case of the elevation dataset, the resulting surface matches (by definition) the earth elevation profile.

Computing a height map in VTK is done with the help of the <strong><u><a href="http://www.vtk.org/doc/nightly/html/classvtkWarpScalar.html">vtkWar</a></u></strong><strong><a href="http://www.vtk.org/doc/nightly/html/classvtkWarpScalar.html">p</a><u><a href="http://www.vtk.org/doc/nightly/html/classvtkWarpScalar.html">Scalar</a></u></strong> filter, whose basic usage is demonstrated in <em>ImageWarp.py</em> (found <u><a href="https://github.com/Kitware/VTK/blob/master/Examples/VisualizationAlgorithms/Python/imageWarp.py">here</a></u><a href="https://github.com/Kitware/VTK/blob/master/Examples/VisualizationAlgorithms/Python/imageWarp.py">)</a>. <strong>Interactivity</strong>

<strong>vtkWarpScalar</strong> provides a <strong>scale factor</strong> that allows the user to control the amount by which the image is moved up or down for a given scalar value at each point: {(x,y),f} is in fact mapped to (x,y,Kf) by the algorithm, where K is the scale factor. This control mechanism can be tied to a <strong>slider GUI</strong> to permit the interactive manipulation of the scale factor and update the visualization accordingly.

<h2>Texture mapping</h2>

Height mapping as described previously produces a geometric representation of the Earth’s surface. In the absence of additional visual cues, however, key aspects of the data, such as the continents’ coast lines, remain ambiguous. To remedy this problem you will use the available satellite image through <strong>texture mapping</strong>.

In VTK, textures are stored in <strong><u><a href="http://www.vtk.org/doc/nightly/html/classvtkTexture.html">vtkTexture</a></u></strong> objects that can then be supplied to <strong><u><a href="http://www.vtk.org/doc/nightly/html/classvtkActor.html">vtkActor</a></u></strong> to enable texture mapping during the rendering stage. Note that this mapping is only possible if the vertices of the target geometry (here, the height surface) are equipped with <em>texture coordinates</em> that specify their matching location on the texture. It is therefore important to point out that the supplied elevation datasets already contain texture coordinates among the point attributes.

<h2>Implementation</h2>

You will write for this task a program that satisfies the following requirements:

Import the necessary files (elevation and satellite image) using the appropriate VTK readers

Compute a height map representation of the elevation with <strong>vtkWarpScalar</strong>

Texture map the satellite image onto the height map geometry

Visualize the result

Provide a slider bar GUI to interactively manipulate the scale factor of <strong>vtkWarpScalar </strong>Maintain consistency between visualization and GUI

<h2>API</h2>

Your program will have following API:

<strong>&gt; heightfield[.py] &lt;elevation&gt; &lt;image&gt;</strong>

where <strong>&lt;elevation&gt;</strong> is the path of the elevation data file and <strong>&lt;image&gt;</strong> is the path of the satellite picture.

<h2>Report</h2>

Include in your report images of the visualization results produced by your program

Make sure to select camera angles that convey the presence of a 3rd dimension

Provide several images showing the results obtained for different scale factors Include close-up images

<h1>Task 2. Isocontours and Color Mapping</h1>

<strong>Task summary</strong>

For the second task, you will <strong>visualize the elevation dataset using isocontours</strong>.

<h2>Isocontours</h2>

To highlight specific values of the elevation, one needs <u><a href="https://en.wikipedia.org/wiki/Contour_line">isocontours</a></u>, which are also known as <em>contour lines</em>, <em>isolines</em>, or more formally <em>level sets</em>. An isocontour corresponds to the set of points where the considered function has a particular value (the <em>pre-image</em> of that function at that value). In 2D, isocontours form closed curves. Note that we will study this topic in great detail when we consider isosurfaces in the coming weeks. By selecting a number of discrete values between the minimum and the maximum values of the data set one can get a good illustration of the value distribution across the domain. To compute isocontours in the elevation dataset you will use <strong><u><a href="http://www.vtk.org/doc/nightly/html/classvtkContourFilter.html">vtkContourFilter</a></u></strong><a href="http://www.vtk.org/doc/nightly/html/classvtkContourFilter.html">,</a> the API of which is discussed <u><a href="https://vtk.org/doc/nightly/html/classvtkContourFilter.html#details">here</a></u>.

Specifically, you will create a visualization that shows isocontours associated with elevation values ranging from -10,000 meters to +8,000 meters in 1,000 meters increments. To improve the clarity of your visualization, you will wrap the isocontour into tubes using <strong><u><a href="http://www.vtk.org/doc/nightly/html/classvtkTubeFilter.html#details">vtkTubeFilter</a></u></strong><a href="http://www.vtk.org/doc/nightly/html/classvtkTubeFilter.html#details">.</a> Note that the radius of your tubes must be selected carefully (not too big to avoid occlusion and not too small to make them easily distinguishable). To facilitate this selection you will create a GUI slider that lets you interactively modify this radius. In addition, you will apply a color map to the isocontours, in other words, assign to each isocontour a color that represents its value. Here, I am asking you to use the color scale shown below, where the <strong>maximal ocean depth</strong> will be mapped to <strong>blue</strong>, the <strong>maximum mountain height</strong> will be mapped to <strong>yellow</strong>, and values close to the sea level (either positive or negative) will be mapped to <strong>desaturated colors</strong>. The <strong>sea level</strong> itself (elevation 0) will be shown in <strong>red</strong>. Note that color maps in VTK are defined through <strong><u><a href="https://vtk.org/doc/nightly/html/classvtkColorTransferFunction.html#details">vtkColorTransferFunction</a></u></strong> that are then supplied to the mapper via <strong>mapper.SetLookupTable(&lt;color_tf&gt;)</strong>, where <strong>&lt;color_tf&gt;</strong> is the name of your color transfer function.

To provide context to the isocontours, you will apply the same texture mapping solution as in the previous task, this time however to a flat geometry. Observe that you may need to tweak the blue and yellow ends of the color map defined above to improve the contrast of your isocontours with the underlying satellite image.

<h2>Implementation</h2>

You will write for this task a program that meets the following requirements: Import the elevation dataset and satellite using the appropriate VTK readers

Define a <strong>vtkContourFilter</strong> to create a series of isocontours in 1,000 meters increments in the interval -10,000 meters to +8,000 meters.

Display the resulting curves as tubes using <strong>vtkTubeFilter</strong>

Color the tubes using the indicated color scale and <strong>vtkColorTransferFunction</strong>

Provide a GUI slider bar to control the radius of the tubes

Texture map a satellite image to the dataset

Keep visualization and GUI selection consistent

<h2>API</h2>

The API for your program should be as follows: <strong>&gt; isocontour[.py] &lt;elevation&gt; &lt;image&gt;</strong>

where <strong>&lt;elevation&gt;</strong> is the path of the elevation data file and <strong>&lt;image&gt;</strong> is the path of the satellite picture.

<h2>Report</h2>

Include in your report high quality images of the results produced by your program

Provide images showing the results obtained for several radius selections Include close-up images

<h1>Task 3. Putting It All Together On A Sphere (30%)</h1>

<h2>Task summary</h2>

For this third and final task, you will <strong>integrate height field representation, texture mapping, and isocontours and display them on a sphere</strong> to create a compelling visualization of the elevation dataset. <strong>Interactive scalar field visualization</strong>

The fourth task in this project consists in combining the various visualization techniques that you implemented previously in a single visualization. Specifically, you should write a program that displays the elevation dataset as a height field, applies to it the satellite imagery using texture mapping (as done in <u>Task 1</u>), and visualizes isocontours as tubes (cf. <u>Task 2</u>). As discussed previously, your visualization should include a slider bar to control the scaling factor of the height field representation. Note that you will set the radius of the isocontours to a fixed value that you found appropriate in <u>Task 2</u>).

<h2>Mapping to a sphere</h2>

In contrast to what you did so far, you will display height field, texture, and isocontours <strong>on a sphere</strong> instead of a plane. Indeed, the results achieved so far do not convey the actual shape of the various continents due to the significant distortion that is caused by the latitude / longitude parameterization of the image. To remedy this problem we need to visualize the data directly on a sphere.

To make your task easier, I am providing you with a sphere dataset that contains elevation values, normals, and texture coordinates. Note that the sphere that I am giving you is already scaled to the radius of the earth (in meters).

<h2>Implementation</h2>

You will write for this task a program that satisfies the following requirements:

Import the elevation dataset on a sphere and satellite image using the appropriate VTK readers

Apply <strong>vtkWarpScalar</strong> to visualize the elevation data as height field

Provide a slider bar to control the scaling factor of <strong>vtkWarpScalar</strong>

Texture map the satellite image onto the height field

Apply <strong>vtkContourFilter</strong> to create a series of isocontours in 1,000 meters increments in the interval -10,000 meters to +8,000 meters

Display the resulting curves as tubes using <strong>vtkTubeFilter</strong>

Color the tubes using the prescribed color map and <strong>vtkColorTransferFunction</strong>

Draw the tubes directly on the height field

Keep visualization and GUI selection consistent

<h2>API</h2>

The API for your program should be as follows:

<h3>&gt; view_earth[.py] &lt;sphere_elevation&gt; &lt;image&gt;</h3>

where <strong>&lt;sphere_elevation&gt;</strong> is the path of the elevation on a sphere data file and <strong>&lt;image&gt;</strong> is the path of the satellite image. <strong>Report</strong>

Include in your report high quality images of the results produced by your program

Provide images showing the results obtained for several scaling factors

Include images that show you results for different parts of the world and different camera perspectives Include close-up images

<h1>Discussion</h1>

Answer the following questions in your report. Be as specific as possible and draw from your experience in this assignment to justify your opinion.

Considering the height map technique used in <u>Task 1</u>:

<ol>

 <li>What properties of the dataset were effectively visualized with this technique?</li>

 <li>What are in your opinion the main limitations of this technique and how could you address them?</li>

 <li>How useful did you find the slider interface in your usage of the height field representation?</li>

 <li>How effective do you find this visualization technique for this dataset?</li>

</ol>

Considering the level sets considered in <u>Task 2</u>:

<ol>

 <li>What specific aspects of the data were readily visible with isocontours?</li>

 <li>How useful did you find the color map and why?</li>

</ol>

Considering the sphere representation in <u>Task 3</u>:

<ol>

 <li>What benefits did you see to the perform the visualization on a sphere?</li>

 <li>How did the resulting visualization compare to the previous ones?</li>

</ol>

Considering your findings in tasks <u>1</u>, <u>2</u>, and <u>3</u>:

<ol>

 <li>Did you find that the combined use of these visualization techniques in <u>Task 3</u> improved upon the results of each technique applied separately? Why or why not?</li>

</ol>

<h1>Datasets</h1>

As indicated in preamble, the data used in this project is courtesy of NASA Earth Observatory and available on the web site of the <u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">Blue Marble pro</a></u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">j</a><u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">ect</a></u><a href="http://visibleearth.nasa.gov/view_cat.php?categoryID=1484">.</a> Since the server can be slow at times, I am providing you with a local copy of the relevant files. Note that I converted the bathymetry / topography datasets to a single <strong>vtkImageData</strong> to simplify your programs. The dataset is available in 4 different resolutions for convenience. Similarly, the satellite image of the Earth is provided in 3 different resolutions. In both cases, use a low resolution version to test your implementation and use for your report the highest resolution that your computer can handle. Note that some of these files are <strong>really large</strong> (see below) so make sure to download only what you need.

<strong>Elevation datasets:</strong>

<u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_small.vti">small version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_small.vti">:</a> 540 x 270, 1.6 MB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_medium.vti">medium version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_medium.vti">:</a> 1080 x 540, 6.0 MB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_large.vti">lar</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_large.vti">g</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_large.vti">e version</a></u>: 2160 x 1080, 24 MB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_very_large.vti">ver</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_very_large.vti">y</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_very_large.vti"> lar</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_very_large.vti">g</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_very_large.vti">e version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_very_large.vti">:</a> 4320 x 2160, 89 MB

<strong>Same elevation datasets on sphere geometry:</strong>

<u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_small.vtp">small version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_small.vtp">:</a> 540 x 270, 9.2 MB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_medium.vtp">medium version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_medium.vtp">:</a> 1080 x 540, 38 MB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_large.vtp">lar</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_large.vtp">g</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_large.vtp">e version</a></u>: 2160 x 1080, 152 MB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_very_large.vtp">ver</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_very_large.vtp">y</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_very_large.vtp"> lar</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_very_large.vtp">g</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_very_large.vtp">e version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/elevation_sphere_very_large.vtp">:</a> 4320 x 2160, 608 MB

<strong>Satellite image:</strong>

<u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.medium.jpg">medium version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.medium.jpg">:</a> 2160 x 1080, 573 KB <u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.large.jpg">lar</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.large.jpg">g</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.large.jpg">e version</a></u>: 5400 x 2700, 2.3 MB

<u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.very_large.jpg">(very</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.very_large.jpg">)</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.very_large.jpg"> lar</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.very_large.jpg">g</a><u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.very_large.jpg">e version</a></u><a href="https://www.cs.purdue.edu/homes/cs530/projects/data/project1/world.topo.bathy.200408.very_large.jpg">:</a> 21600 x 10800, 36 MB

<h1>Report</h1>

Please refer to the <u><a href="https://www.cs.purdue.edu/homes/cs530/syllabus.html">class s</a></u><a href="https://www.cs.purdue.edu/homes/cs530/syllabus.html">y</a><u><a href="https://www.cs.purdue.edu/homes/cs530/syllabus.html">llabus</a></u> for late policy and formatting guidelines of project reports. Your report should also contain your answers to the questions asked in the <u>discussion section</u>. As a reminder, <strong>this project must be completed individually</strong>.


