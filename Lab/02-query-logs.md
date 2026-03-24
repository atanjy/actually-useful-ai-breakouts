# 2.2. Query Logs

Let's now dive into some Loki querying, using Loki's LogQL query syntax.

## Run a Loki query and view logs in a Table

1.  In the main logs panel, open the context menu (three dots, at the top right of the panel, located near the _Logs / Table / JSON_ toggle), then click on **Open in Explore**.

    Grafana automatically turns your Logs Drilldown session into a Grafana Explore session, filling in your LogQL query for you, so you can begin deeper analysis of the logs.

    Notice how we can see:

    - The LogQL query that was being used to perform our search

    - The log volume over the current time period

> [!Tip]
> LogQL is the query language in Loki. A basic LogQL query looks like this:
>```
> {my_label="value"} |= `foo`
>```


2.  Modify the LogQL query to remove the `status_code` and `http_method` label filters, as below. Then, click on **Run query**:

    ```
    {service_name=`web_app_3`} | json |~ `(?i)favicon\.ico`
    ```

    _Label filters_ (`{label="value"}`) are the building blocks of any LogQL query. They allow Loki to find streams of logs.

    This new query:

    - Finds all logs which have the label `service_name` equal to `web_app_3`.

    - Parses each log line to extract JSON fields.

    - Filters the logs for lines containing the string `favicon.ico`

3.  In the main **Logs** panel, click on the **Table** toggle in the right hand corner, if it is not already selected.

    We can use the Table view to show logs in a tabular format, for easy scanning and analysis.

    Labels, and fields that have been detected from the logs, are shown in the _Fields_ panel on the left hand side.

4.  In the **Fields** panel, check the boxes by **status_code** and **http_method**.

    Now we can see columns for these two fields, allowing us to understand the logs more easily. The column _Line_ shows the original log line:

    <img width="1546" height="747" alt="image" src="https://github.com/user-attachments/assets/c7438109-53a6-4812-a36c-8d368371fd24" />

>[!Tip]
>From here, you can also download the logs in text, JSON or CSV formats by clicking on the **Download** button.

Notice how we've been able to shape and explore our data at query time, using Loki's query-time JSON parser, and without having to index the content of the logs upfront!
