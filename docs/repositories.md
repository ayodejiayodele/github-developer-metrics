# Data Transformation for repositories

## Transformation Steps

Each of these steps represents a transformation applied to the data in the Power Query Editor.

1. **Step 1**: `Source = Organizations` - This line sets the source of the data to be the "Organizations" table.

2. **Step 2**: `#"Invoked Custom Function" = Table.AddColumn(Source, "repositories", each GetRepos(Text.Replace([repos_url],GitHubAPI,"")))` - This line adds a new column named "repositories" to the "Source" table. The values in this column are obtained by invoking the `GetRepos` function on the `repos_url` column after replacing the `GitHubAPI` string with an empty string.

3. **Step 3**: `#"Expanded repositories" = Table.ExpandListColumn(#"Invoked Custom Function", "repositories")` - This line expands the "repositories" column, which is a list, into multiple rows.

4. **Step 4**: `#"Expanded repositories2" = Table.ExpandRecordColumn(#"Expanded repositories", "repositories", {"Column1"}, {"repositories.Column1"})` - This line expands the "repositories" column, which is a record, into multiple columns. The new column is named "repositories.Column1".

5. **Step 5**: `#"Renamed Columns" = Table.RenameColumns(#"Expanded repositories2",{{"repositories.Column1", "repositories"}})` - This line renames the "repositories.Column1" column to "repositories".

6. **Step 6**: `#"Expanded repositories1" = Table.ExpandListColumn(#"Renamed Columns", "repositories")` - This line expands the "repositories" column, which is a list, into multiple rows.

7. **Step 7**: `#"Expanded repositories3" = Table.ExpandRecordColumn(#"Expanded repositories1", "repositories", {...}, {...})` - This line expands the "repositories" column, which is a record, into multiple columns. The new columns are named with the prefix "repositories." followed by the original field name.

8. **Step 8**: `#"Filtered Rows" = Table.SelectRows(#"Expanded repositories3", each ([repositories.name] <> ".github" and [repositories.name] <> ".github-private"))` - This line filters the rows of the table, keeping only those where the "repositories.name" is not equal to ".github" and ".github-private".

9. **Step 9**: `#"Expanded repositories.owner" = Table.ExpandRecordColumn(#"Filtered Rows", "repositories.owner", {"login"}, {"repositories.owner.login"})` - This line expands the "repositories.owner" column, which is a record, into multiple columns. The new column is named "repositories.owner.login".

10. **Step 10**: `in` - This keyword is used to specify the result of the Power Query expression.

11. **Step 11**: `#"Expanded repositories.owner"` - This line specifies that the result of the Power Query expression is the table obtained after expanding the "repositories.owner" column.

## Power Query

```json
let
    Source = Organizations,
    #"Invoked Custom Function" = Table.AddColumn(Source, "repositories", each GetRepos(Text.Replace([repos_url],GitHubAPI,""))),
    #"Expanded repositories" = Table.ExpandListColumn(#"Invoked Custom Function", "repositories"),
    #"Expanded repositories2" = Table.ExpandRecordColumn(#"Expanded repositories", "repositories", {"Column1"}, {"repositories.Column1"}),
    #"Renamed Columns" = Table.RenameColumns(#"Expanded repositories2",{{"repositories.Column1", "repositories"}}),
    #"Expanded repositories1" = Table.ExpandListColumn(#"Renamed Columns", "repositories"),
    #"Expanded repositories3" = Table.ExpandRecordColumn(#"Expanded repositories1", "repositories", {"id", "node_id", "name", "full_name", "private", "owner", "html_url", "description", "fork", "url", "forks_url", "keys_url", "collaborators_url", "teams_url", "hooks_url", "issue_events_url", "events_url", "assignees_url", "branches_url", "tags_url", "blobs_url", "git_tags_url", "git_refs_url", "trees_url", "statuses_url", "languages_url", "stargazers_url", "contributors_url", "subscribers_url", "subscription_url", "commits_url", "git_commits_url", "comments_url", "issue_comment_url", "contents_url", "compare_url", "merges_url", "archive_url", "downloads_url", "issues_url", "pulls_url", "milestones_url", "notifications_url", "labels_url", "releases_url", "deployments_url", "created_at", "updated_at", "pushed_at", "git_url", "ssh_url", "clone_url", "svn_url", "homepage", "size", "stargazers_count", "watchers_count", "language", "has_issues", "has_projects", "has_downloads", "has_wiki", "has_pages", "has_discussions", "forks_count", "mirror_url", "archived", "disabled", "open_issues_count", "license", "allow_forking", "is_template", "web_commit_signoff_required", "topics", "visibility", "forks", "open_issues", "watchers", "default_branch", "permissions", "security_and_analysis"}, {"repositories.id", "repositories.node_id", "repositories.name", "repositories.full_name", "repositories.private", "repositories.owner", "repositories.html_url", "repositories.description", "repositories.fork", "repositories.url", "repositories.forks_url", "repositories.keys_url", "repositories.collaborators_url", "repositories.teams_url", "repositories.hooks_url", "repositories.issue_events_url", "repositories.events_url", "repositories.assignees_url", "repositories.branches_url", "repositories.tags_url", "repositories.blobs_url", "repositories.git_tags_url", "repositories.git_refs_url", "repositories.trees_url", "repositories.statuses_url", "repositories.languages_url", "repositories.stargazers_url", "repositories.contributors_url", "repositories.subscribers_url", "repositories.subscription_url", "repositories.commits_url", "repositories.git_commits_url", "repositories.comments_url", "repositories.issue_comment_url", "repositories.contents_url", "repositories.compare_url", "repositories.merges_url", "repositories.archive_url", "repositories.downloads_url", "repositories.issues_url", "repositories.pulls_url", "repositories.milestones_url", "repositories.notifications_url", "repositories.labels_url", "repositories.releases_url", "repositories.deployments_url", "repositories.created_at", "repositories.updated_at", "repositories.pushed_at", "repositories.git_url", "repositories.ssh_url", "repositories.clone_url", "repositories.svn_url", "repositories.homepage", "repositories.size", "repositories.stargazers_count", "repositories.watchers_count", "repositories.language", "repositories.has_issues", "repositories.has_projects", "repositories.has_downloads", "repositories.has_wiki", "repositories.has_pages", "repositories.has_discussions", "repositories.forks_count", "repositories.mirror_url", "repositories.archived", "repositories.disabled", "repositories.open_issues_count", "repositories.license", "repositories.allow_forking", "repositories.is_template", "repositories.web_commit_signoff_required", "repositories.topics", "repositories.visibility", "repositories.forks", "repositories.open_issues", "repositories.watchers", "repositories.default_branch", "repositories.permissions", "repositories.security_and_analysis"}),
    #"Filtered Rows" = Table.SelectRows(#"Expanded repositories3", each ([repositories.name] <> ".github" and [repositories.name] <> ".github-private")),
    #"Expanded repositories.owner" = Table.ExpandRecordColumn(#"Filtered Rows", "repositories.owner", {"login"}, {"repositories.owner.login"})
in
    #"Expanded repositories.owner"

```
