# Project Documentation

##Architecture Breakdown
![Data model explanation (1)](https://github.com/user-attachments/assets/495339af-8f03-46b0-ad3a-f610ca89d95f)

## Front End Overview
- Create ReactApp
- Call many API
- Profit

## BackEnd Overview

This project is designed to consume Grasshopper files, parse their component data, and store the information in a TinkerPop graph database. This allows for version control and analysis of Grasshopper files.

## Project Structure

### PluginGrasshopper.csproj

**Description:**
This is the project file for the Grasshopper plugin. It includes references to common settings and libraries required for the project.

**Responsibilities:**
- Defines the project configuration, including target framework and dependencies.
- Manages build settings and output paths.
- Ensures that all necessary libraries and packages are included for the project to compile and run correctly.

### Gremlin.cs

**Description:**
This file contains the `GremlinConnector` class, which is responsible for connecting to the TinkerPop graph database using Gremlin.

**Responsibilities:**
- Establishes a connection to the TinkerPop graph database.
- Provides methods to begin and commit transactions.
- Adds and retrieves test objects from the graph database.
- Adds nodes to the graph database.

### GraphStrutObject.cs

**Description:**
This file contains the `GraphStrutObject` class, which represents a collection of various nodes related to a graph structure in Rhino.

**Responsibilities:**
- Loads a Grasshopper document and parses its components.
- Iterates through document objects to extract component data.
- Processes connected objects and converts bitmap images to Base64 strings.
- Manages the creation of nodes representing component definitions, instances, inputs, and outputs.

### GremlinGeneric.cs

**Description:**
This file contains the `GremlinGeneric` class, which implements the `IGremlinGeneric` interface.

**Responsibilities:**
- Provides generic methods to interact with the graph database.
- Adds nodes and connections to the graph.
- Finds nodes and checks for the existence of nodes and connections in the graph.

### DigestGHFile.cs

**Description:**
This file contains the `DigestGHFile` class, which is a Rhino command that processes a Grasshopper file.

**Responsibilities:**
- Executes the command to load and parse a Grasshopper file.
- Uses the `GraphStrutObject` class to extract component data.
- Adds the parsed data to the TinkerPop graph database using the `GremlinGeneric` class.
- Manages the creation and connection of nodes in the graph database.

### CommonVariables.bat

**Description:**
This batch file sets up common environment variables used in the project.

**Responsibilities:**
- Sets the path to the Visual Studio Developer Command Prompt tools.
- Determines the path to the `yak` executable based on the installed version of Rhino.
- Sets the name of the solution to build.
- Specifies the directory where resulting `yak` packages will be copied.

**Example:**

'@echo off

REM VsDevTools might need to be adapted according your installation of Visual Studio 
set "VsDevTools=C:\Program Files\Microsoft Visual Studio\2022\Community\Common7\Tools\VsDevCmd.bat"

REM where to find yak
if exists "C:\Program Files\Rhino 8\System\yak.exe" (
    set "YakExecutable=C:\Program Files\Rhino 8\System\yak.exe"
) else (
    set "YakExecutable=C:\Program Files\Rhino 7\System\yak.exe" 
) 

REM name of the solution to build
set "Name=PluginTemplate"

REM where to copy resulting yak packages
set "YakTargetDir=bin\packages"
'


## Usage

### Loading a Grasshopper File

1. The `DigestGHFile` command is executed in Rhino.
2. The command opens a file dialog to select a Grasshopper file.
3. The selected file is loaded using the `GraphStrutObject` class.
4. The document objects are iterated and processed to extract component data.

### Adding Data to the Graph Database

1. The parsed component data is added to the TinkerPop graph database using the `GremlinGeneric` class.
2. Nodes and connections are created based on the component definitions, instances, inputs, and outputs.

### Example Code

#### DigestGHFile.cs



## Detailed Documentation for GraphStrutObject.cs

### Class: GraphStrutObject

#### Properties

- **ComponentDefinitionNodes**: A dictionary that stores component definition nodes. Each node contains information about a specific component definition.
- **ComponentInstanceNodes**: A dictionary that stores component instance nodes.
- **InputNodes**: A dictionary that stores input nodes associated with the `GraphStrutObject`.
- **OutputNodes**: A dictionary that stores output nodes in the `GraphStrutObject`.
- **DocumentVersionNode**: A node to store document version information.
- **DocumentNode**: A node to store document information in the GraphHop system.

#### Private Fields

- **_ioDoc**: An instance of `GH_DocumentIO` used to load the Grasshopper document.
- **_ghDoc**: An instance of `GH_Document` representing the loaded Grasshopper document.
- **_xmlDoc**: An instance of `XmlDocument` representing the XML representation of the Grasshopper document.
- **_versionId**: A `Guid` representing the version ID of the document.

#### Methods

##### LoadDocument
public bool LoadDocument(string filePath, out string errmsg)
**Description**: Loads a Grasshopper document from the specified file path.

**Parameters**:
- `filePath`: The path to the Grasshopper file.
- `errmsg`: An output parameter that returns an error message if the document fails to load.

**Returns**: `true` if the document is loaded successfully; otherwise, `false`.

##### IterateDocumentObjects
public void IterateDocumentObjects()

**Description**: Iterates through all document objects and processes them.

##### GetConnectedObjects
public void GetConnectedObjects(IGH_DocumentObject obj, ComponentInstanceNode componentInstanceNode)

**Description**: Processes the connected objects (inputs and outputs) of a given document object.

**Parameters**:
- `obj`: The document object to process.
- `componentInstanceNode`: The component instance node to update.

##### PopulateDocumentProperties
private void PopulateDocumentProperties()
**Description**: Populates the properties of the document node.

##### PopulateDocumentObjectProperties
public ComponentInstanceNode PopulateDocumentObjectProperties(IGH_DocumentObject obj)
**Description**: Populates the properties of a document object and returns a component instance node.

**Parameters**:
- `obj`: The document object to process.

**Returns**: The populated component instance node.

##### ProcessInput
private void ProcessInput(IGH_Param source, ComponentInstanceNode componentInstanceNode)
**Description**: Processes an input source and updates the component instance node.

**Parameters**:
- `source`: The input source to process.
- `componentInstanceNode`: The component instance node to update.

##### ProcessOutput
private void ProcessOutput(IGH_Param recipient, ComponentInstanceNode componentInstanceNode)
**Description**: Processes an output recipient and updates the component instance node.

**Parameters**:
- `recipient`: The output recipient to process.
- `componentInstanceNode`: The component instance node to update.

##### ConvertBitmapToBase64
private string ConvertBitmapToBase64(Bitmap bitmap)
**Description**: Converts a Bitmap image to a Base64 string.

**Parameters**:
- `bitmap`: The Bitmap image to convert.

**Returns**: The Base64 string representation of the Bitmap image.

### Referenced Structures

#### ComponentDefinitionNode

**Description**: Represents a node containing information about a specific component definition.

**Properties**:
- `ComponentGuid`: The unique identifier of the component.
- `Name`: The name of the component.
- `Description`: The description of the component.
- `Icon`: The Base64 string representation of the component's icon.

#### ComponentInstanceNode

**Description**: Represents a node containing information about a specific component instance.

**Properties**:
- `InstanceGuid`: The unique identifier of the instance.
- `ComponentGuid`: The unique identifier of the component.
- `NickName`: The nickname of the instance.
- `X`: The X-coordinate of the instance's position.
- `Y`: The Y-coordinate of the instance's position.
- `Inputs`: A list of input node GUIDs.
- `Outputs`: A list of output node GUIDs.
- `XmlRepresentation`: The XML representation of the instance.

#### DataInputNode

**Description**: Represents a node containing information about an input connection.

**Properties**:
- `TargetGuid`: The unique identifier of the target node.
- `InstanceGuid`: The unique identifier of the input instance.
- `NickName`: The nickname of the input.
- `Name`: The name of the input.

#### DataOutputNode

**Description**: Represents a node containing information about an output connection.

**Properties**:
- `TargetGuid`: The unique identifier of the target node.
- `InstanceGuid`: The unique identifier of the output instance.
- `NickName`: The nickname of the output.
- `Name`: The name of the output.

#### DocumentVersionNode

**Description**: Represents a node containing information about the document version.

**Properties**:
- `VersionId`: The unique identifier of the document version.

#### DocumentNode

**Description**: Represents a node containing information about the document.

**Properties**:
- `DocumentID`: The unique identifier of the document.
- `DisplayName`: The display name of the document.
- `FilePath`: The file path of the document.
- `Owner`: The owner of the document.
- `DocumentHeadersXml`: The XML representation of the document headers.
- `DefinitionPropertiesXml`: The XML representation of the definition properties.
- `RcpLayoutXml`: The XML representation of the RCP layout.
- `GHALibrariesXml`: The XML representation of the GHA libraries.
