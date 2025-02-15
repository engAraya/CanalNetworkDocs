# Layout plans and Network Resolution

Refer to the standard workflow for using CanalNETWORK software. One of the input data is a layout map in AutoCAD. This section discusses the task of importing such a layout map from an AutoCAD environment, and processing the connectivity data for the network of canals. Most of the task is handled automatically, while it needs some assistance from the user to resolve some issues that may linger.

## AutoCAD canal layout maps

The AutoCAD layout maps must fulfil certain conventions to be used in the CanalNETWORK software environment. The following are essentially assumed to be true in the AutoCAD source drawing:

1. All routes are drawn using Polyline command. Any object drawn with the Line command can not be processed, and will be excluded. It causes error on subsequent tasks.

2. All canal routes are drawn from upstream to downstream directions. 

3. All canal routes start and end with a stratight segment of at least 20m. 
   
   ![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20001.png) *Maintain straight segments at begining and end of canal alignments*

4. All canal routes are drawn to scale of 1:1, and referenced to world coordinate system (WCS) of AutoCAD.

Best practices for layout drawings include, but are not limited to:

1. Organization by layer: it is very helpful to collect canal routes of similar generation or level to one layer. Users have, while working on a number of projects, observed that this will greately enhance key batch tasks later. For instance, collect all TC (teritiary canals) to a separate layer, say *TC Canals*. Similarly, collect all quaternary level canals to a separate layer called, say, *QC Canals*. And so on.

2. Intersection of canals: The points where any two canals intersect will form a junction node. Nodes constitute the basic data block for network analysis. Therefore, all canals must be drawn with care - especially near the begining and the end. The following are key recommendations:
   
   * where two canals offtake from a single parent canal, make sure they intersect the parent canal at the same point.
   
   * Where two canals offtake from a single parent canal, make sure they do not intersect themselves any where other than the point where they intersect the parent canal. This may be the case where the starting segments of the branch canals are either parallel, or the cross the parent canal at slighly different locaitons. If necessary edit the first or second vertext of one of the canal routes to avoid parallel.
   
   ![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20002.png) * Intersection of branch canals with a parent canal*
* If possible avoid curves on a parent canal where it intersects with branch canals.

* Except where brnaching is needed, avoid extending the end of canals anywhere close to other canals, and certainly not with in the extension distance (specified in Network Preferences). Failure to observe this will result in unintended junctions leading to significant time in correcting the connectivity of the network.
  
  ![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20003.png) 
  
  *Desired network relation ship.*
  
  ![[](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20004.png) *Unintended Junction at a canal end potentially confusing canal parent-child relation ships*
3. All routes should end at sufficient distance from other canal routes. (Sufficiently farther than the extension length and node merge distance parameter values in `Network Preferences`, which are often set at 2m and 5m respectively.) 

> Note: The amount of time spent to resolve a network analysis, depends little on the size of the irrigation area or the number of canals. Rather it greately depends on how strictly the above guideline is adhered to in preapring the layout map.

For this reason, it is highly recommended that the engineer responsible for preparing layout maps must be aware of these guiding rules and assumptions.

## Network resolution: Definition

Network resolution is a term used to represent the task of analysing the connectivity between each canal route in the canal network system, and ensuring that each canal in the network has ONLY one parent canal.

Where this basic criteria is not met across the system of canals, the network is NOT resolved, and progress to the subsequent steps (for instance Network establishing in the standard workflow) is bound to abort with an error of some sort.

After a succesful network resolution task, the user can do the following:

1. Automatically generating ordered and sequentila naming for all routes in the network.
   
   Some canals may need to be manually named depending on specific needs of the project. These canals can be assigned NAME EXCEPTIONS. A common example is a TC branching from another TC. This route will be automatically named a QC, but the user can assign in a TC level as an exception.

2. Plot these namings to AutoCAD environment with desired text specification (height, font,...)

3. Use the resolved network structure to group canal routes by generation and instance them in AutoCAD, for batch processing at a later stage.

Lets now see how layout maps prepared in AutoCAD are imported to CanalNETWORK environment and analyzed for connectivity.

## Importing layout map to CanalNETWOK environment

The first step is to import the drawing to the CanalNETWORK environment. There are many ways to do this:

1. To import a individual routes - i.e., a single route at a time - Go to `Workspace > Pick Route (AutoCAD)` .
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20005.png)

2. To import alignment routes drawn on a layer, go to Workflow > Pick Routes (AutoCAD Layer). This will invoke a dialog listing all the layers in the current drawing. 
   
   ![[  ] ](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20007.png)

   Pick the desired layer, and hit OK. This will import all alignment routes found on the specified layer to the plan view area.

   Note: This method is recommended for drawings whose layers are well organized, especially for the different levels of canals expected in the project.

3. One can also import a collection of canal routes at once. To do this:
   
   * First, create an instance of all the canal routes on a host object with in the AutoCAD environment, using the AutoCAD addon tool.
   
   * Then go to `Workspace > Pick Rotue (AutoCAD)` menu command, and when prompted pick the host object.
     
     ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20006.png)
     
     This will import all the canal routes instanced in the host object. 
     
     ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20009.png)

> Note: A complete canal route data consists an alignment data (x, y) and a profile data. The below $Partial Import?$ prompt may apear when importing canal routes whose profile data is not yet extracted. This is perfectly acceptable, and proceed by hitting `Partial` button. Partial meanse that only the alignment data will be available with out the profile data. Also, note that routes imported with partial are represented with broken lines in the plan veiw area of the interface.

![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20010.png) 

*Routes in plan view area showing canal routes with complete data set, and partial data set.*

Every time, the routes are generated drawn in the plan view area of the interface.

Remember, alignmnet routes that are NOT drawing as polyline, and whose length is less than 20M will be filtered out, and hence can not be imported.

### Removing Unwanted Routes

Unwanted polyline objects may be imported to the plan view area during the process of importing canals. Also, the user may want to remove a canal route and re-import it again. To remove such objects:

1. Select the objects to remove in the layout view (plan view) area. If needed, enable multi-select from` Edit > Multi Select (Routes)` to allow selection of multiple routes at a time.

2. Use `Edit > Remove Route` menu command.
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20008.png)

![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20011.png) *Delete dialog requesting confirmation for selected routes.*

Once all the canal routes are imported, the next step is Node Identification.

## Node Identification

The term Node is used to represent a location on a layout map that represents key locations along canal routes. There are three types of nodes.

1. Canal junciton nodes: are created where a branch canal meets a parent canal. These are represented as circles. These are represented as Circles.

2. End of Canal Nodes: are created where a canal route terminates with out connection to a branch canal. Such nodes are represented as squares. These are represented as squares.

3. Floating Nodes: are created by the user to represent any changes in physical or hydraulic, or other information along a canal route. This types of nodes are not relevant for the current topic of Network Resolution, but are discussed in detail in the section [Longitudinal Design of Routes](). 

![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20012.png) 

*Figure showing appearance of Junction Nodes and End-of-Canal nodes in layout view area.*

Note: The size of the nodes as they apear on the plan view can be customized in Node graphic.

CanalNETWORK can automatically analyze all imported canals to identify and insert nodes as needed. Node identification is invoked from` Workflow > Nodes > ID Nodes` menu command.

> Note: It is recommended to change the view to layout view. The other parts of the interface are not of much use at this stage.

1. To identify nodes for selected routes, first select the desired routes and start the command.
   
   ![[   ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20014.png) 
   
   *Figure showing the confirm dialog before Node Identification process.*
   
   This will invoke the *Confirm* dialog requesting confirmation on the number of canal routes currently selected for the process. Click `Yes `to continue.

2. To Identify nodes for the entire network of canals, make sure there are no selected routes in layout view area (right click and hit `Clear Selection` ). Then start the command. 
   
   ![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20015.png) 
   
   *Figure showiing confirm dialog before Node Identification process for entire network.*
   
   This will invoke the confirm dialog as above. Please note the number of rotues to be addressed in the dialog to differentiate between the two cases.

The process will automatically generate Nodes for the desired routes and display them.    The generated nodes now represent parent and branch canals at every junction in the network. Often, the connectivity between canal routes will not be perfect to desired conditions. This is ALWAYS because the conventions and assumptions listed early in this section are not observed fully during layout preparation in AutoCAD drawings.

Most of these connectivity issues can be identified automatically, as discussed below.

## Identifying Connectivity Issues and Resolving them

The user may expect two types of isses with the connectivity established from above step.

### Unresolved EoC nodes

The first common issue is locations where End-of-Canal nodes are inserted, instead of a Junction node. This condition misses to understand that there is a branch canal leaving from the end of the parent canal, impairing further work during canal design. It must be resolved.

![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20016.png) 

*Wrongly created EOC Node instead of a Junction node.*

There are two approaches to resolve such issues. Manual and Automatic methods. The manual method allows to resolve individual issues.

1. First right-click on the branch canal to select it.

2. Then left-click on the EOC node to be corrected. This will invoke the *Edit Node* dialog, detailing the current connectivity status at the specific node.
   
   ![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20017.png) 
   
   *Edit Node Dialog for the EoC node*

3. Click on the` To Current Node` button to link the picked route to the node. This will correct the issue, also changing the square node marker to a circle node marker correctly representing a canal junction.
   
   ![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20018.png) 
   
   *Resolved Junction Node at End of Canal.*

Oftern however, there are too many EoC nodes that must be corrected or resolved in a network. This is especially true after the first attempt to ID nodes for a complete network of routes. The automatic tool is helpful in this situation. To use it, go to `Workflow > Nodes > Convert EoC Nodes `menu command. 

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20019.png)

![[  ] ](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20020.png)

*Confirm Operation dialog.*

This will identify all wrongly positioned EoC Nodes in the entire network database, and correct the issue one-by-one, also informing the user of the progress. 

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20021.png)

![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20022.png)

*Progress bar and message at end of operation informing the user of how many EoC nodes are succesfully converted.*

### Mulitple Parent Nodes

The key purpose of node resolution is to ensure that each branch canal has only one parent canal. However, some canal arrangements may mislead the software and assign two routes as parents for a single brnach canal. Again, this ALWAYS happens when the assumptions and conventions listed early in this section are not fully observed during layout preparation in the AutoCAD environment.

CanalNETWORK can automatically locate these nodes in the network. The user must manually resolve the issue.

To identify nodes where multiple parent condition may be an issue, go to Workflow > Nodes > Diagnose Multiple-Parent Nodes. If found, the command will list such locations in a table.

![[  ]](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20023.png) 

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20024.png)

*Menu command and resulting table for Multiple Parent Node issue identification. The table shows both First and Second Nodes refering to the same branch canal (not shown in the table)*

Once this is ready, the user can navigate to each location and resolve the issue. To go to a particular node listed in the table:

![[  ] ](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20025.png)

*Go to Route menu command*

Select the row for the node, and use` View > Go To Route...` menu command (Ctrl + G for keyboard short cut.) The layout view will pan and zoon in to the desired node location.

In the table list of nodes shown above, both nodes refer to the one branch canal. This can be noted by left-clicking on each node, one at a time. It can be seen that the *Edit Node* dialog lists the same branch canal to each node. 

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20026.png)

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20027.png)

Obviously, the lower-right node is the issue. This is because, it is the end of a canal and no branch is expected or implied in the drawing. What caused this issue is the fact that the canal in question ends very close to the begining of the other canal. The AutoCAD drawing measurement confirms this.

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20028.png)

 *AutoCAD measured distance between end of a canal and begining of another canal. Based on the preference (shown to the right) search radius for a neighbouring canal is 2x2meters (4meters) locating the wrong canal as a parent canal.*

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20029.png)

To resolve the issue:

1. Left-Click on the route for which multiple parents are assigned. It will be selected.

2. Left-Click on the mistaken Node (lower right inthis case). This will invoke the *Edit Node* dialog. 

3. Click on the `Remove Cur` button. This action will do two things: detach the branch canal from the node, and convert the node to EoC node.

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20030.png)

The issue is resolved. Repeat the above steps for all other such nodes.

Before continuing to the next step, Re run the test `Workflow > Nodes > Diagnose Multiple-Parent Nodes...`  to verify all issues are succesfully resolved, or new issues are not created in the process.

## 

## Generating Canal Naming for the Resolved Network

Generating a naming - cross-referenced - labels for each canal route in the network is a true test of fully resolved network, that guarantees smooth workflow in subsequent network analysis and design tasks.

To create a desired naming for the network of canals just resolved, first a naming style is needed. 

## Defining Canal Naming Style

There are multiple styles available to generate a canal naming, depending on the project type, scale and user preference. To create a naming style go to `Workspace  > Manage Design Criteria > New Namiing..`.  

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20031.png)

This will start the *Edit Variables* Dialog, set each variable to desired values as follows:

1. Canal Network Naming: This prescribes the exact sequence of naming to be followed begining from the primary level (1st generation) canal all the way to the last level of of canals. 
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20032.png)
   
   *Available options for Canal Naming*

2. Level II Desc. Text: Defines the text to be used 2nd level (2nd generation) canal rotues in the network. 
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20033.png)
   
   *Available Options for Descriptive texts*

3. Over-ride Min. Discharge: The setting for this variable is overwritten in the design criteria. Hence users may leave this to the default value of -1 (representing None)

4. Next Action: On Apply Use For: leace as is

5. New Descriptive Label: Optional string to spefify the naming style defined, to help easily idenfity for latter use.

> Note: All inputs must be selected from the avaiable options, and no custom inputs are accepted. 

As a best practice, it is always recommended to use a naming with sufficient levels (at least a few levels longer than expected ) to label all generations in the network. Otherwise, erros will arise further lengthenning the tiime taken to resolve the network.

Upon completion, hit the `Apply` button. This will invoke the Get Design Criteria? dialog. It is essential to confirm to this dialog with the `Yes` button.

![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20034.png)

*Get Design Criteria dialog*

This will import the default design criteria set for the network data, which can latter be altered to meet specific design conditions.

Creating the naming list is completed here. The next step is to attempt to generate canal naming to each route, and resolve any remaining issues in the network.

### Create Naming

To create names for each route in the canal network, based on the naming style just defined (above):

1. Select the primary level canal for the project by left-clicking on it in the layout view area.
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20036.png)
   
   If a naming style is not defined yer, the below dialog will terminate the process. Define a naming style and try again.
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20035.png)

2. Go to View > Route Text > Create New Text...  A process dialog such as shown below may apear. Choose `AutoStack and Repeat` until all issues are handled, or the number of issued does not reduce any more.
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20037.png)

3. Choose *Tag* on the dialog, and hit `OK`. 
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20038.png)

   A naming style is generated per specifications, and applied to each route. A table also lists the naming applied to each route ID.

   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20039.png)

**A network data is accepted as fully resolved if all routes are succesfully named with appropriate level indices. **

Beware of ** strings. These indicate routes whose connections have not been resolved fully in earlier node identification and resolution process.  In the screenshot above, for instance, the last route has an issue. It can be seen that the connecting node to the parent canal envisages and EoC node, and not a branch junction node. This must be resolved to get the correct naming. Pan and zoom in the entire network area to see if there are any ** labels and resolve any issues causing them.

*FInd Text dialog invoked using Ctrl + F keyboard combination.*

Use `Ctrl` + `F` keyboard combination to search for such texts in large networks. If found, they will be highlighted for easily locating them.



### Rendering Names to AutoCAD

The canal namimng generated as described above can be exported to the AutoCAD environment to label each source alignment object of the canal network accordingly. To export naming:

1. Make sure the desired text is displayed on the layout view area. Toggle the text view button on if not already. Use `View > Route Text > Select and Shot Text...` menu command if not already.
   
   ![dasd ](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20041.png)*Sample Layout map with Canal naming displayed.*
   
   

2. Choose `View > Route Text > Generate to AutoCAD` menu command. This will invoke the *Sample Text* dialog.
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20042.png)
   
   
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20043.png)

3. Choose `Continue` button. AutoCAD environment is now in select mode waiting for user input. Pick a sample text. 
   
   ![s](C:\Users\Dell\Documents\GitHub\CanalNetworkDocs\Layout%20plans%20and%20Network%20Resolution\Images\Image%20044.png)
   
   All texts are generated to AutoCAD environemnt using the text height of the sample text selected above. Other similar information, for instance Area and Discharge capacity, can also be generated using this method.
   
   

END.
