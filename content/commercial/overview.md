+++
widget = "text_image"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = true  # This file represents a page section.
active = true  # Activate this widget? true/false
weight = 10  # Order that this section will appear in.

title = "Versions and Features"
+++

<style>
table {
    width: 80%;
    margin: 1rem auto 3rem auto;
}
@media screen and (max-width: 576px) {
  table {
    width: 100%;
    margin: 0;
  }
}
table th:first-of-type {
    width: 10%;
}
table th:nth-of-type(2) {
    width: 33%;
}
table th:nth-of-type(3) {
    width: 30%;
    text-align: center;
}
table th:nth-of-type(4) {
    width: 30%;
    text-align: center;
}
</style>

**Eclipse MOSAIC** is the free-to-use and open-source version of the **MOSAIC** simulation framework. This version
includes all simulators and tools we think are most valuable for research purposes. For more complex studies, we provide additional
features in our **MOSAIC Extended** version, which includes all open-source elements of Eclipse MOSAIC and additional tools for scenario
preparation and visualization. For commercial users we even provide additional simulators, such as the vehicle simulation **PHABMACS**.

If you are interested in MOSAIC Extended please leave us a message at mosaic@fokus.fraunhofer.de and we will
create an individual offer suitable to your needs.


|               | *Feature*                                                                     |              {{< img src="/img/logos/mosaic/EclipseMOSAIC-Logo-RGB-positiv.svg" width="220px" >}}               | {{< img src="/img/logos/mosaic/MOSAICExtended-Logo-RGB-positiv.svg" width="220px" >}} |
|:--------------|:------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------:|:-----------:|
|               | **License Type**                                                              | Open Source <br> [EPL 2.0](https://www.eclipse.org/legal/epl-2.0/)<br>[**Attribution required**](/publications) | Commercial |
|               |                                                                               |                                                                                                                 |             ||| 
|               | **[Runtime Infrastructure](/docs/extending_mosaic)**                          |                                                      **✓**                                                      |    **✓**    |
|               | **[Interactions Library](/docs/extending_mosaic/interactions)**               |                                                      **✓**                                                      |    **✓**    |
|               | **[Tutorials](/tutorials/)**                                                  |                                                      **✓**                                                      |    **✓**    |
| *Simulators*  |                                                                               |                                                                                                                 |             || 
|               | **[Application Modelling](/docs/simulators/application_simulator)**           |                                                      **✓**                                                      |    **✓**    |
|               | **[Eclipse SUMO](/docs/simulators/traffic_simulator_sumo)**                   |                                                      **✓**                                                      |    **✓**    |
|               | **[OMNeT++](/docs/simulators/network_simulator_omnetpp)**                     |                                                      **✓**                                                      |    **✓**    |
|               | **[ns-3](/docs/simulators/network_simulator_ns3)**                            |                                                      **✓**                                                      |    **✓**    |
|               | **[Cellular Communication](/docs/simulators/network_simulator_cell)**         |                                                      **✓**                                                      |    **✓**    | 
|               | **[ITS-G5 Communication](/docs/simulators/network_simulator_sns)**            |                                                      **✓**                                                      |    **✓**    |
|               | **[Environment](/docs/simulators/environment_simulator)**                     |                                                      **✓**                                                      |    **✓**    |
|               | **[Battery](/docs/simulators/emobility_simulator_battery)**                   |                                                        -                                                        |    **✓**    |
|               | **[Charging Station](/docs/simulators/emobility_simulator_charging)**         |                                                        -                                                        |    **✓**    |
|               | **[Variable Message Signs](/docs/simulators/vms_simulator)**                  |                                                        -                                                        |    **✓**    |
|               | **CARLA**                                                                     |                                                     _soon_                                                      |   _soon_    |
|               | **PHABMACS Scenario API**                                                     |                                                     _soon_                                                      |    **✓**    |
|               | **[PHABMACS Vehicle Simulator](/docs/simulators/vehicle_simulator_phabmacs)** |                                                        -                                                        |    **✓**    |
| *Visualizers* |                                                                               |                                                                                                                 |             || 
|               | **[2D Visualizer](/docs/visualization)**                                      |                                                      **✓**                                                      |    **✓**    |
|               | **[File Output](/docs/visualization/filevis)**                                |                                                      **✓**                                                      |    **✓**    |
|               | **[Statistics Output](/docs/visualization/statistics)**                       |                                                        -                                                        |    **✓**    |
|               | **[3D Visualizer](docs/visualization/3d-vis)**                                |                                                        -                                                        |    **✓**    |
| *Tools*       |                                                                               |                                                                                                                 |             ||| 
|               | **[Command Line Tool](/docs/getting_started/run_mosaic)**                     |                                                      **✓**                                                      |    **✓**    |
|               | **Run Federates in Docker**                                                   |                                                      **✓**                                                      |    **✓**    |
|               | **[scenario-convert](/docs/scenarios/scenario_convert)**                      |                                                      (✓)*                                                     |    **✓**    |
|               | **[Simulation Runner](docs/scenarios/run_simulation_series)**                 |                                                        -                                                        |    **✓**    |
|               | **TrafficGen**                                                                |                                                        -                                                        |    **✓**    |

*) **scenario-convert** is not part of the open-source version of Eclipse MOSAIC, but can be freely used.