# Hurricane Evacuation Modeling
This project is a baseline for running an agent-based model to simulate evacuation movements in Charleston County, South Carolina. A short literature review and background to this project can be found [here](https://github.com/taiyangie/MATSim/blob/master/Hurricane_Evac.pdf). The following is a guide for using the developer version in a Java IDE, which is necessary to access the code and libraries to generate your own scenario files. Both the GUI-only and developer version will use identical GUIs and require the same skeleton files.

### Running the GUI
To first open the project in IntelliJ on a Linux machine, run the following:
```
cd / 
cd opt
jetbrains/jetbrains-toolbox
```

Assuming all the setup for MATSim has been completed and the project is open in IntelliJ, this section will cover how to run a baseline simulation. To start up the GUI for MATSim, go to src>main>java>org.matsim>gui>MATSimGUI. Three .XML files are necessary in order to run a full simulation: a config, network, and population file. 

### Creating the Config File

The config file is the file that is directly input into the MATSim GUI. This file contains the locations of the network and population files, the number of iterations and length of day to simulate, activity start and end times, and flow and storage capacities. The following example is a 1% test run of the current scenario. When running large and new scenarios, it is best practice to start at 1%.
```
<module name="qsim">
   <param name="startTime" value="00:00:00"/>
   <param name="endTime" value="30:00:00"/>
   <param name="flowCapacityFactor" value="0.01"/>
   <param name="storageCapacityFactor" value="0.01"/>
</module>
```

### Creating the Network File

A network consists of nodes and links
```
<nodes>
   <node id="103775899" x="456283.916991751" y="5733496.654420585" />
   <node id="107672423" x="454341.19496910257" y="5734567.121554276" />
</nodes>
<links capperiod="01:00:00" effectivecellsize="7.5" effectivelanewidth="3.75">
   <link id="1" from="31559588" to="31559589" length="77.30198648491773"
      freespeed="8.333333333333334" capacity="600.0" permlanes="1.0"
      oneway="1" modes="car" origid="16275017" />
   <link id="10" from="448010227" to="121904163" length="510.8378636333411"
      freespeed="12.5" capacity="600.0" permlanes="1.0" oneway="1"
      modes="car" origid="38089606" />
</links>
```
The standard method of creating a MATSim starts with downloading the most recent stable build of Osmosis, which can be found at http://wiki.openstreetmap.org/wiki/Osmosis

From here, download the relevant .osm file from https://download.geofabrik.de for your region of interest. Then, you'll need to go to https://www.openstreetmap.org/ to find the coordinates that will define a box around the location from where you want to extract your road network. To do so, zoom into you region and hit the export button in the top left corner. Note this bounding box. In the command line, type the below command with your bounding box coordinates in order to extract the road network.

```
   java -cp osmosis.jar --rb file=xxx.osm.pbf \
--bounding-box top=47.701 left=8.346 bottom=47.146 right=9.019 \ completeWays=true --used-node --wb allroads.osm.pbf
```             

Sometimes it also makes sense to extract major roads not confined in this specific region. To do so, use the following command:

```
   java -cp osmosis.jar --rb file=xxx.osm.pbf --tf accept-ways \ highway=motorway,motorway_link,trunk,trunk_link,primary,primary_link \ --used-node --wb bigroads.osm.pbf
```

If you choose to extract these major roads, the two network files can be merged as follows:

```
     java -cp osmosis.jar --rb file=bigroads.osm.pbf --rb allroads.osm.pbf \ --merge --wx merged -network.osm
```

### Creating the Population File

A population file contains synthetic people who each have a plan. Each plan has activities and legs which describe what each agent does and how they move.

```
<person id="12052000_12052000_0">
   <plan selected="yes">
      <act type="home" x="454788.14217382803" y="5735115.491572791"
         end_time="07:16:20" />
      <leg mode="car"></leg>
      <act type="work" x="451947.00656881003" y="5736160.56555245"
      end_time="14:55:41" />
      <leg mode="car"></leg>
      <act type="home" x="454788.14217382803" y="5735115.491572791"
      end_time="16:22:14" />
      <leg mode="car"></leg>
      <act type="shopping" x="455006.52115909086" y="5734909.411959048"
      end_time="17:41:13" />
      <leg mode="car"></leg>
      <act type="home" x="454788.14217382803" y="5735115.491572791" />
   </plan>
</person>
```

A census or survey is often used to create a synthetic population for the area. Additionally, facility data needs to be extracted from the area (schools, workplaces, homes). Finally, behavior data is needed in the form of travel diaries and commuting distances. For each 

Continue reading for information on how to use MATSim.

# Setting up MATSim

This is a guide to setting up MATSim on your machine.

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



# Sources
TU Berlin's virtual MATSim course: https://isis.tu-berlin.de/course/view.php?id=31123
Simunto help docs:https://www.simunto.com/matsim/tutorials/eifer2019/slides_day2.pdf
Simunto VIA: https://docs.simunto.com/via/getting-started/first-visualization.html
Initial MATSim input generation: https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwixnLSjmd7-AhUKlWoFHeNPA0EQFnoECBAQAQ&url=https%3A%2F%2Fwww.ubiquitypress.com%2Fsite%2Fchapters%2F10.5334%2Fbaw.7%2Fdownload%2F1475%2F&usg=AOvVaw0a-2vUiZd0-C4rEiy7t0yJ


