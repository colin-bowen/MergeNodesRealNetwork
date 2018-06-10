# MergeNodesRealNetwork
need to merge nodes and write them into TEPES

%the goal is to successfully sort the names of nodes and generators based
%on geographical location. Begin by plotting the two sets of vertices to
%observe trends using gplot. Then implement a conditional tolerance
%statement to join node names with corresponding generators. there are 304
%nodes with geographical location, and 113 generation nodes with location.
%The script output will be the names of the generators paired with the name
%of the nodes, with the coordinates of the nodes being used to approximate
%the ´center´ of the city.

%call file and name it and defining node indices
filename = "SpanishPortuguese_400kV_Location_PowerGrid";
sheetnodos = "Nodos-Localizacion";
node_name = xlsread(filename,sheetnodos,"A1:A304");
node_id = xlsread(filename, sheetnodos, "B1:B304");
node_location = xlsread(filename, sheetnodos, "C1:D304");

%define generation indices
generation = "Generación";
generation_name = xlsread(filename, generation, "A2:A113");
generation_id = xlsread(filename, generation, "B2:B113");
generation_location = xlsread(filename, generation, "D2:E113");

%preallocate matrix size for adjacency matrix
adjacency_matrix_nodes = ones(length(node_id));
adjacency_matrix_generators = ones(length(generation_location));

%will have node ids
node_from = xlsread(filename, "Líneas", "B2:B440");
node_to = xlsread(filename, "Líneas", "F2:F440");

%adds connections where they are found
%adjacency_matrix_nodes(node_from, node_to) = 1;

%plots graph on nodes
figure
gplot(adjacency_matrix_nodes, node_location, '.r');
hold on
gplot(adjacency_matrix_generators, generation_location, ".g");
