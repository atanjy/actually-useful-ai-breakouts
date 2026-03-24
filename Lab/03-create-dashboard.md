# 3.2. Build a dashboard with Loki

## Save a new dashboard

Let’s create our first Loki dashboard using label and metrics extraction queries.

1. Click the plus button in the top right of Grafana to create a new dashboard.

2. Select **last 3 hours** in the time picker (top right)

3. Click **Save dashboard** and give your dashboard a name.

## Add a Geomap panel 
We're now going to add a panel showing a Geomap using the country code that was added by geocoding the IP address. 

1. Ensure you're in Edit mode in your dashboard. (From Grafana 11 onwards, you need to click the **Edit** button in the top right corner.)

2. Click **Add** -> **New visualization**.

3. Select the **LokiNGINX** datasource.

4. Paste the following query, which counts the log lines, grouped by the extracted country_code:

    ```
    sum by (geoip_country_code) (count_over_time({filename="/var/log/nginx/json_access.log"} | json | __error__="" [1m]))
    ```

5.  Underneath the query box, click **Options** to expand the options panel, and set the **Legend** value to `{{geoip_country_code}}`.

6.  Above the query, click on the **Transformations** tab, then **Add transformation**. 

7.  From the transformations palette, add the **Reduce** transformation, and select the **Series to rows** Mode. For the **Calculations** field, select **Total**.

8.  In the sidebar, click on the dropdown at the top and change the panel type to **Geomap**.

9.  In the **Panel options** sidebar, change the following settings:

    - Under **Map layers**, change the **Layer type** to **ArcGIS MapServer**.
    - Click the **Add layer** button and add a new layer of type **Markers**. This will add a new layer above the existing layer. It's important for visualization that the ArcGis MapServer layer is shown underneath the Markers.
    - Click on the new **Markers** layer, and:
        - change **Location Mode** from **Auto** to **Lookup**
        - change the **Lookup field** to **Field**
        - Make sure the **Gazetteer** field is **Countries**
        - Make sure **Styles Size** field is **Total**, with Min 10 and Max 40
        - Make sure **Color** is **Fixed Color** and pick the **red** color
        - Set **Fill opacity** to **0.8**

    - Change the panel title to **Total requests per country**.

10.  Return to your dashboard and **save** your progress.

## Add more panels
For a guide to adding more panel types, refer to optional lab: [optional-add-more-panels.md](docs/optional-add-more-panels.md)
