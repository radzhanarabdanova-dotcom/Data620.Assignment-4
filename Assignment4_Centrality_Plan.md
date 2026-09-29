# Assignment 4: Centrality Measures — Project Plan

## Data Source

**GitHub REST API** — `https://api.github.com`

- Endpoint `/users/{username}/followers` and `/users/{username}/following` to collect connections between developers.
- Endpoint `/users/{username}/repos` to determine each user's primary programming language.
- The API is free and does not require complex authentication for small-scale requests (a personal access token increases the rate limit but is not strictly required).

## Network Description

- **Nodes:** GitHub users (developers)
- **Edges:** directed "follows" relationship between two users (A follows B)
- **Categorical variable:** Primary programming language of each user, derived from the language of their most-starred repository (e.g., Python, JavaScript, Java, Go, Other)

## Plan for Loading the Data

1. Select a seed list of ~20–30 active developers — for example, the top contributors to a well-known open-source repository (e.g., TensorFlow, React, or a repository relevant to data science).
2. For each seed user, query the GitHub API for their followers and following lists, keeping only connections that fall within the seed set (building an ego-network rather than the entire GitHub graph, which is far too large).
3. For each user in the resulting node set, query their repositories and record the language of their most popular (most-starred) repo as the categorical attribute.
4. Store the results as two CSV files:
   - `edges.csv` with columns `source, target`
   - `nodes.csv` with columns `user, primary_language`
5. Load both files into a graph analysis library (e.g., NetworkX in Python) for further processing in later assignments.

## Hypothesized Outcome

Developers with **higher degree centrality** (more followers/following within the sampled network) are expected to cluster around the most widely-used and currently in-demand languages (e.g., Python, JavaScript), since these ecosystems have the largest active communities and the most cross-project collaboration.

Developers working in smaller, more specialized languages (e.g., Rust, Go) are expected to show **lower degree centrality** overall, but some of these developers may show **relatively higher betweenness centrality**, acting as bridges between niche language communities and the broader, more mainstream developer network.

If this pattern holds, degree centrality could serve as a rough proxy for a developer's visibility/reach within the general open-source community, while betweenness centrality would better highlight developers who connect otherwise-separate technical communities.
