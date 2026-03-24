# Optional: Correlate metrics, logs, and traces

> [!NOTE]
> Ensure you have Prometheus and Tempo data sources added to your Grafana instance. Refer to [01-connect-data-sources.md](Lab/01-connect-data-sources.md)

Labels in Loki work like labels in Prometheus, allowing you to efficiently query, filter, and organize your log data. In Grafana, you can correlate logs with metrics using these labels, enabling you to view related data side-by-side, and gain deeper insights into your system's behavior and performance.

1.  From the Grafana main menu, choose **Explore** and then from the data source picker, select the **PromCorrelation** datasource:
    
    <img width="1368" height="932" alt="image" src="https://github.com/user-attachments/assets/cd3f357e-2562-42bf-a6e4-c78e2130dd0d" />

2. Click on the **Metrics browser** button. Then, from the Metrics Browser, under **Select a metric**, enter **web** and then choose **web_http_requests** from the list of metrics.

    Then click on **Use query**.

3. Make sure the Time Picker (right upper corner, clock icon) is set to **last 5 minutes**.
    
    Notice the three series. Each series charts the number of concurrent users over time. 
   
4. Filter down on the suspect service, `web_app_3`, by modifying the query to add a label filter:

    ```
    web_http_requests{service="web_app_3"}
    ```
 
    Then click on **Run query** or press Shift+Enter to run.

    Notice the suspect 'saw pattern'? Let's find out what's causing the steep drop in concurrent users requests.

## Correlating Prometheus metrics with Loki logs

1. Click the split button next to the time picker.
    
    <img width="2406" height="1516" alt="image" src="https://github.com/user-attachments/assets/d2fffa5d-95df-414d-9f61-445fdca1256f" />

2. The new (right) panel shows the same Prometheus query. Change the data source selection on the right-hand panel to our **LokiCorrelation** data source. 
    
    Notice that Grafana recognizes the label selection of Prometheus, and applies the same labels to your Loki logs query, so you see the latest relevant logs right away.
  
3. Click the chain button next to the time picker to sync up the time range of both views. If it is already selected, it will be highlighted in orange.

   <img width="1558" height="483" alt="image" src="https://github.com/user-attachments/assets/4c5124ef-df7f-4419-afdf-f7008e934ba9" />

  
4. In the left-hand Prometheus panel, select the timerange where the drop happened, by clicking and dragging a time period with your mouse.
    
   <img width="1070" height="844" alt="image" src="https://github.com/user-attachments/assets/75b1d709-489e-4914-90c2-38ada8646441" />


    Notice the Loki panel time range is synced, and reloads logs from the newly selected time period.
  
5. If you see a lot of log lines, add an additional filter to show only the errors, by changing the Loki query to:

    ```
    {service="web_app_3", error_level="ERROR"}
    ```

    You should see a log line showing "out of memory", which hints at a memory leak being the reason of the drop in concurrent users.

    ```
    [ERROR] out of memory error. Dying...argh
    ```

## Correlating Loki logs with traces

Besides outages, it’s also common to have latency issues for which traces allows us to find the bottleneck in complex request/response flows. 

1. Zoom out the time range until you have log lines that start with a `tempo_trace_id`.

    To zoom out, you can use the magnifying glass button.
 
2. Click on a log line with a **Trace ID**
      
3. In the parsed fields, next to the Trace ID value, click on the blue **Tempo** or **View traces** button:

    <img width="1700" height="1044" alt="image" src="https://github.com/user-attachments/assets/f55b5dfd-6c8a-45f3-9af3-e273a0eeebeb" />

4. Grafana opens the Tempo trace view for that trace.

    You can now inspect the trace in the Grafana Trace View and understand any possible bottlenecks in the request/response flow of your service.
