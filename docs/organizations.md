# Data Transformation for organizations

## Transformation Steps

1. **Step 1**: `Source = Table.FromList(Text.Split(OrganizationList,","))` - This line sets the source of the data to be a table created from a list. The list is obtained by splitting the `OrganizationList` string at each comma.

2. **Step 2**: `#"Renamed Columns" = Table.RenameColumns(Source,{{"Column1", "organization"}})` - This line renames the "Column1" column in the "Source" table to "organization".

3. **Step 3**: `#"Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"organization", type text}})` - This line changes the data type of the "organization" column to text.

4. **Step 4**: `#"Added Custom" = Table.AddColumn(#"Changed Type", "repos_url", each "https://api.github.com/orgs/"&[organization]&"/repos")` - This line adds a new column named "repos_url" to the table. The values in this column are obtained by concatenating the string "https://api.github.com/orgs/", the value in the "organization" column, and the string "/repos".

5. **Step 5**: `#"Added Custom1" = Table.AddColumn(#"Added Custom", "copilot_url", each "https://api.github.com/orgs/"&[organization]&"/copilot")` - This line adds another new column named "copilot_url" to the table. The values in this column are obtained by concatenating the string "https://api.github.com/orgs/", the value in the "organization" column, and the string "/copilot".

6. **Step 6**: `in` - This keyword is used to specify the result of the Power Query expression.

7. **Step 7**: `#"Added Custom1"` - This line specifies that the result of the Power Query expression is the table obtained after adding the "copilot_url" column.

## Power Query

```json

let
    Source = Table.FromList(Text.Split(OrganizationList,",")),
    #"Renamed Columns" = Table.RenameColumns(Source,{{"Column1", "organization"}}),
    #"Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"organization", type text}}),
    #"Added Custom" = Table.AddColumn(#"Changed Type", "repos_url", each "https://api.github.com/orgs/"&[organization]&"/repos"),
    #"Added Custom1" = Table.AddColumn(#"Added Custom", "copilot_url", each "https://api.github.com/orgs/"&[organization]&"/copilot")
in
    #"Added Custom1"

```
