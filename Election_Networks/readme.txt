State Networks from:
Reuning, Kevin "Mapping Influence: Partisan Networks Across the United States,
  2000 to 2016." State Politics and Policy Quarterly

Contact info:
Email: Kevin.Reuning@gmail.com
Website: www.kevinreuning.com

Software used: R 3.5.1

NOTE: THIS DATA IS ESTIMATED FOR FULL ELECTION CYCLES. SENATE NETWORKS
  CONTAIN MULTIPLE ELECTIONS UNTIL THE WHOLE CHAMBER HAS BEEN
	RE-ELECTED.

#### Folder/File Information ###
- Edge_List
  ## Edge list for all state networks, see line 74 for details.
- Metadata
  ## Node level data for all state networks, see line 74 for details.
- Aggregate_Data
- state_list.csv - CSV with the election-chamber-cycle years.
- network_stats.csv ## Stats on network level characteristics. Each stat
    ## estimated on backbone includes 5 versions for different levels of
    ## of cutoffs. See information on backbone networks below.


#### Loading and Using Networks #####
As discussed in the article, networks are developed for each
state-chamber-full-cycle. The data for the networks are split between the edge
lists and the node level data (it is hard to combine the two while making it
easily accessible by a variety of statistical software).

## Edge list
  Each csv file in Edge_Lists represents 1 network. The first two columns are
  the sender/receiver identified with their "EID" (the identifier provided by
  NIMP). The third column is used to identify what threshold the edge was
  identified for. For robustness there were 5 thresholds estimated: .9, .95,
  .975, .99, .995. The number in that column goes from 1 to 5 and reflects each
  threshold. If an edge is a 3, then it means it was identified at the .975
  threshold. When identifying edges you should use all edges at that level and
  above.

  The networks in the article used the .975 threshold and so edges greater than
  or equal to 3 were included.
## Metadata
  Each csv file in Metadata contains the node level attributes for a network.
  EIDs are given to match them. The other variables are:
    ContributorName: Name of contributor
    CatCodeIndustry: Industry as identified by NIMP
    CatCodeGroup: Group as identified by NIMP
    CatCodeBusiness: Business as identified by NIMP
    PerDem: Percent donated to Democratic candidates that full-cycle
    PerRep: Percent donated to Republican candidates that full-cycle
    DemCol: Color used for nodes based on PerDem
    RepCol: Color used for nodes bad on PerRep
    Total: Total donated that full-cycle.

## Loading networks in R.
If you are interested in creating a network in R the following code will do it
with the .975 backbone and all of the node level data. This is for the Colorado
lower house in the 2013 to 2014 cycle.

library(igraph)
net <- read.csv("Edge_Lists/CO-2013-2014-House.csv")
meta_df <- read.csv("Metadata/CO-2013-2014-House.csv")
net <- net[net$edge>=3,]
net <- as.matrix(net[,1:2])
net <- graph_from_data_frame(net, vertices=meta_df, directed=F)
