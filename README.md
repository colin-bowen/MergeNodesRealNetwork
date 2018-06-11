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

%implement algorithm for nearest search. Iteratively, find closest node to generator
%by finding the euclidean distance between the points. Reject and reconsider points that
are more than 1km away

%generates Euclidean distances of all generators from each node location
for i = 1:length(generation_location) %check each generator
  for j = 1:length(node_id) %check each generator against node location
    euc_dist(i,j) = sqrt((generation_location(i,1)-node_location(j,1))^2+(generation_location(i,2)-node_location(j,2))^2);
  end
end

%selects minimum distance for each generator, and records the index of the 
min_dist = zeros(1,length(generation_location));
for i = 1:length(generation_location)
  [min_dist(i),index_min(i)] = min(euc_dist(i,:));
end

%converts degrees to kilometers for maximum tolerance
dist_km = deg2km(min_dist);


%moves generation location to the node_location with minimum index
for i = 1:length(generation_location)
  if dist_km < 10
  new_generation_location(i) = node_location(index_min(i));
end

hold off
figure
gplot(adjacency_matrix_generators, new_generation_location, '.c')
