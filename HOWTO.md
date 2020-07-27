How to easily create a gitlab/github Helm Charts repository

It is very simple and easy to setup your own personal Helm Charts repository. All you need is a gitlab/github repo, both public and private works.

At the top of the repo you need a file called "index.yaml". It can be auto-generated with the Helm CLI.
"helm repo index <path/to/charts/parent/directory>"

This will produce a "index.yaml" file. This file contains metadata about all the charts that was located in the path given to "helm repo index <path>", and it is used by helm to indentify and search for the charts to fetch and install.

If you want to create your own Chart and upload it to your Helm Chart repo, then you first need to create the chart.

A Chart is a directory with different configuration files inside. Inside of this directory, Helm will expect a structure that matches this:

{code:bash}
chart-name/
  Chart.yaml          # A YAML file containing information about the chart
  LICENSE             # OPTIONAL: A plain text file containing the license for the chart
  README.md           # OPTIONAL: A human-readable README file
  values.yaml         # The default configuration values for this chart
  values.schema.json  # OPTIONAL: A JSON Schema for imposing a structure on the values.yaml file
  charts/             # A directory containing any charts upon which this chart depends.
  crds/               # Custom Resource Definitions
  templates/          # A directory of templates that, when combined with values,
                      # will generate valid Kubernetes manifest files.
  templates/NOTES.txt # OPTIONAL: A plain text file containing short usage notes
{code}

After you have created the chart, move the root folder of the chart to the directory where you store all your charts, aka <charts/dir>
Then run 

{code:bash}
helm package <charts/dir/your-new-chart>
helm repo index <charts/dir>
{code}

And now you have updated the index.yaml file with the new packaged chart.

To add a Helm Chart repo:

Private github/gitlab repositry:
{code:bash}
helm repo add - username <your_github_username> - password <your_github_token> <helm-chart-repo-name> 'https://raw.githubusercontent.com/my_organization/my-github-helm-repo/master/'
{code}
Public
{code:bash}
helm repo add <helm-chart-repo-name> https://raw.githubusercontent.com/my_organization/my-github-helm-repo/master/
{code}

Then run:
{code:java}
helm repo update
helm repo list
helm search <helm-chart-repo-name>
{code}

Now you should see something like this:
{code:java}
NAME CHART VERSION APP VERSION DESCRIPTION
helm-chart-repo-name/my-helm-chart 0.1.0 1.0 A Helm chart for Kubernetes
{code}
