# Hurricane Evacuation Modeling
This project is a baseline for running an agent-based model to simulate evacuation movements in Charleston County, South Carolina.

### Running the GUI
Assuming all the setup for MATSim has been completed and the project is open in IntelliJ, this section will cover how to run a baseline simulation. To start up the GUI for MATSim, go to src>main>java>org.matsim>gui>MATSimGUI. Three .XML files are necessary in order to run a full simulation: a config, network, and plans file. 

### Creating the Config File

### Creating the Network File

The standard method of creating a MATSim starts with downloading the most recent stable build of Osmosis, which can be found at http://wiki.openstreetmap.org/wiki/Osmosis

From here, download the relevant .osm file from https://download.geofabrik.de for your region of interest. Then, you'll need to go to https://www.openstreetmap.org/ to find the coordinates that will define a box around the location from where you want to extract your road network. To do so, zoom into you region and hit the export button in the top left corner. Note this bounding box. In the command line, type the below command with your bounding box coordinates in order to extract the road network.

```
   java -cp osmosis.jar --rb file=xxx.osm.pbf \
--bounding-box top=47.701 left=8.346 bottom=47.146 right=9.019 \ completeWays=true --used-node --wb allroads.osm.pbf
```             

Sometimes it also makes sense to extract major roads not confined in this specific region. To do so, use the following command:

'''
   java -cp osmosis.jar --rb file=xxx.osm.pbf --tf accept-ways \ highway=motorway,motorway_link,trunk,trunk_link,primary,primary_link \ --used-node --wb bigroads.osm.pbf
'''

If you choose to extract these major roads, the two network files can be merged as follows:

'''
     java -cp osmosis.jar --rb file=bigroads.osm.pbf --rb allroads.osm.pbf \ --merge --wx merged -network.osm
'''

### Creating the Plans File


Continue reading for information on how to use MATSim.

# matsim-example-project

A small example of how to use MATSim as a library.

By default, this project uses the latest (pre-)release. In order to use a different version, edit `pom.xml`.

A recommended directory structure is as follows:
* `src` for sources
* `original-input-data` for original input data (typically not in MATSim format)
* `scenarios` for MATSim scenarios, i.e. MATSim input and output data.  A good way is the following:
  * One subdirectory for each scenario, e.g. `scenarios/mySpecialScenario01`.
  * This minimally contains a config file, a network file, and a population file.
  * Output goes one level down, e.g. `scenarios/mySpecialScenario01/output-from-a-good-run/...`.
  
  
### Import into eclipse

1. download a modern version of eclipse. This should have maven and git included by default.
1. `file->import->git->projects from git->clone URI` and clone as specified above.  _It will go through a 
sequence of windows; it is important that you import as 'general project'._
1. `file->import->maven->existing maven projects`

Sometimes, step 3 does not work, in particular after previously failed attempts.  Sometimes, it is possible to
right-click to `configure->convert to maven project`.  If that fails, the best thing seems to remove all 
pieces of the failed attempt in the directory and start over.

### Import into IntelliJ

`File -> New -> Project from Version Control` paste the repository url and hit 'clone'. IntelliJ usually figures out
that the project is a maven project. If not: `Right click on pom.xml -> import as maven project`.

### Java Version

The project uses Java 11. Usually a suitable SDK is packaged within IntelliJ or Eclipse. Otherwise, one must install a 
suitable sdk manually, which is available [here](https://openjdk.java.net/)

### Building and Running it locally

You can build an executable jar-file by executing the following command:

```sh
./mvnw clean package
```

or on Windows:

```sh
mvnw.cmd clean package
```

This will download all necessary dependencies (it might take a while the first time it is run) and create a file `matsim-example-project-0.0.1-SNAPSHOT.jar` in the top directory. This jar-file can either be double-clicked to start the MATSim GUI, or executed with Java on the command line:

```sh
java -jar matsim-example-project-0.0.1-SNAPSHOT.jar
```



### Licenses
(The following paragraphs need to be adjusted according to the specifications of your project.)

The **MATSim program code** in this repository is distributed under the terms of the [GNU General Public License as published by the Free Software Foundation (version 2)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html). The MATSim program code are files that reside in the `src` directory hierarchy and typically end with `*.java`.

The **MATSim input files, output files, analysis data and visualizations** are licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" /></a><br /> MATSim input files are those that are used as input to run MATSim. They often, but not always, have a header pointing to matsim.org. They typically reside in the `scenarios` directory hierarchy. MATSim output files, analysis data, and visualizations are files generated by MATSim runs, or by postprocessing.  They typically reside in a directory hierarchy starting with `output`.

**Other data files**, in particular in `original-input-data`, have their own individual licenses that need to be individually clarified with the copyright holders.


