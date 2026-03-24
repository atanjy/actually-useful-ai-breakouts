# 2.1. Queryless - Exploring logs

## View logs from a service

The _Logs Drilldown_ app in Grafana provides an easy way to browse, search and filter Loki logs. When you're using the app, you can quickly find the logs you need without having to write a query.

In this lab, we'll look at some essential features of Logs Drilldown that help you find the logs you need, quickly. Some users have been reporting missing favicons on their websites, so let's see if we can find out why.

1. From the left menu, expand the **Drilldown** menu item and click on **Drilldown** -> **Logs**.

    The Logs Drilldown app opens with the service list. This list is populated with all the services that are sending logs to Loki, grouped by the label `service_name`. This helps you to quickly find the service you want to investigate.

   <img width="1920" height="1350" alt="image" src="https://github.com/user-attachments/assets/faeb5860-8b8d-47df-adab-312830294bee" />

> [!TIP]
> If you can't see any logs, then ensure you have the **LokiCorrelation** data source selected in the top right corner of the screen.

3. Scroll down to the `web_app_3` service and click the **Show logs** button to open the logs for this service.

    <img width="1872" height="300" alt="image" src="https://github.com/user-attachments/assets/2d805c4b-f825-4c8e-8a1b-1c1cfb68fc31" />

4. Now we see the most recent logs for our app **web_app_3**. This view shows us:

    - A chart showing the volume of logs received for this service over time

    - A list of log lines for this service

    - The time period for search, shown in the top right

    <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9035b0fa-baf7-4673-8253-ed5700d7126a" />

> [!TIP]
>   If the log lines are too long for your browser, you can scroll horizontally, or click the **Wrap** button to enable line wrapping.

## Understanding a log line in Loki

1. While viewing the logs for **web_app_3**, **click on one of the returned log lines** to expand it.

2. When you expand a log line, the detail view shows the **Fields** that are associated with it:

    - _Indexed labels_ which locate the log line in Loki's index (denoted by **I**)

    - _Parsed labels_ which are fields inside the log line itself, that Loki has parsed at query time (denoted by **P**)

    - _Structured metadata_ which are un-indexed key-value pairs attached to a log line (denoted by **S**)
    
    The fields are displayed in a table, with icons that allow you to filter the logs by that field.

    <img width="1501" height="696" alt="image" src="https://github.com/user-attachments/assets/a23ce00b-4a8f-4103-83f0-35d5b946259e" />

## Searching and filtering

1. Let's troubleshoot our website by finding all the requests for the `favicon.ico` file that are returning a 404 status code.

    In the search bar, enter the text **favicon**. This will show only those log lines containing the string `favicon`:

    <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/cd499daa-ab6d-402f-a160-25c46a07c9df" />


2. Let's add another filter for status_code.

    **Click on a log line** to expand it. Then, by the side of **status_code**, click on the magnifying glass icon with the plus sign: 
    
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-hidden="true" width="16" height="16" class="css-fmaj2t-Icon">
    <rect x="-2" y="-2" width="28" height="28" fill="white"/><path d="M15,10H12V7a1,1,0,0,0-2,0v3H7a1,1,0,0,0,0,2h3v3a1,1,0,0,0,2,0V12h3a1,1,0,0,0,0-2Zm6.71,10.29L18,16.61A9,9,0,1,0,16.61,18l3.68,3.68a1,1,0,0,0,1.42,0A1,1,0,0,0,21.71,20.29ZM11,18a7,7,0,1,1,7-7A7,7,0,0,1,11,18Z"></path></svg>

    <img width="1167" height="490" alt="image" src="https://github.com/user-attachments/assets/a001c5b4-30e0-491f-a0a3-24f662ced6ea" />


3. The log results are updated to show only those logs which contain the string `favicon` **and** have the `status_code` that you just selected.

    At the top of the page, we can see all of our search filters, where we can easily change or remove them:

    <img width="1920" height="525" alt="image" src="https://github.com/user-attachments/assets/db29e20d-187b-4501-a3d5-bda793fbec61" />


4. Let's change our status_code filter to show only 404 responses. At the top of the screen, click on the **status_code** label filter and pick the **404** value from the dropdown list.

    Now, only logs from the app which contain the string `favicon.ico` and have the status_code **404** are shown. We're already on our way to solving the mystery of the missing favicon!

    <img width="1920" height="975" alt="image" src="https://github.com/user-attachments/assets/523a878e-3ea6-450e-b2bf-e6d25b6dbf35" />

## Optional: View metrics from logs

Logs Drilldown also lets you deeply understand the shape and content of your logs, through instant metrics and charts. 

Under the hood, Loki's _metrics from logs_ feature instantly calculates metrics from your log lines or labels, which Logs Drilldown then visualizes as a chart.

This helps you to quickly answer very common questions, like:

- Is the number of errors in my app increasing?
- Which are the most popular routes in my app, or the most popular pages on my website?

Let's take a look at these metrics:

1. From the row of tabs above the log volume chart, click on the **Labels** tab.

    Logs Drilldown shows a breakdown of Loki labels, charting their respective values.

    In the **http_method** panel, click on the **Select** button to drill down to logs with this label:

    <img width="1915" height="842" alt="image" src="https://github.com/user-attachments/assets/70b43f3d-560d-49be-bb95-7363ace02e7a" />


2. Now we can see a further breakdown of our filtered logs, broken down by the `http_method` label.

    From any panel, click on the **Include** button to show only logs with that label value.

    <img width="1917" height="815" alt="image" src="https://github.com/user-attachments/assets/02b04fef-6f18-492c-a1f5-3c64b9aa55cc" />


3. Then click the **Logs** tab to return to the logs list.

    Notice how the logs have been further filtered to show only those which have your selected `http_method` label value.

    <img width="1512" height="765" alt="image" src="https://github.com/user-attachments/assets/a3d527b8-43f2-48c9-9fc6-86a3eb396114" />


## Wrapping up

Logs Drilldown is a powerful tool for diving into your logs and gaining instant insights without having to write a query.

When you want to dive further into Loki, you can start writing queries in _LogQL_, Loki's query language. In the next section of this lab, we will move from Logs Drilldown into writing queries for Loki.
