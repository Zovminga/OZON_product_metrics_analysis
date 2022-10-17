# OZON analysis pet project
Pet project with data analysis and product metrics from OZON.  
There is block with SQL queries, building seller journey map with different sales models comparison and calculating main product metrics to sales analysis  

## Project blocks:
### I. SQL block  
  
Ad-hoc queries from different tables used in OZON - "SQL block OZON.md" file  
-----------------------------------------------
### II. Case study block

**INFO:**
1. Illustrating the process of sellers' journey on the Marketplace platform from the moment of registration to the 1st sale according to the DBS sales model.  
Description of the main differences from the FBO and FBS sales models

2. Finding the main difficulties that may appear at each stage from the moment of registration to the first sale in the DBS model path;

3. Developing main metrics that can show the key differences in the processes between mentioned sales models;

4. Setting KPIs for metrics and justifying the relevance and adequacy of these KPIs

**Additional information**

The OZON marketplace offers sellers several sales models:

**FBO (Fulfilment by OZON)** – the seller stores goods in the marketplace warehouse. The marketplace itself is responsible for warehouse  
processing and logistics of the order to the client. The income of the marketplace consists of a commission for a showcase, warehouse operations and logistics.  

**FBS (Fulfilment by seller)** – the seller independently stores the goods in his own or choosen warehouses. He is responsible for warehouse handling.  
After receiving the order in the Personal Account (on marketplace website), the seller processes it and transfers it to the Marketplace logistics.  
The income of the marketplace consists of a commission for a showcase and logistics.  

**FBS 2.0 / Real FBS / DBS (Delivery by seller)** – the seller independently stores the goods in his warehouse. He is responsible for warehouse handling.  
After receiving the order in the Personal Account, the seller processes it and transfers it by any logistics convenient for him.  
The income of the site consists of the commission for the showcase.  

**HOW ITS DONE**  
Journey map is done in Miro board. You can find link in the file in repo or click [here](https://miro.com/app/board/uXjVPNNncKo=/).  
Also the map is available as an image in the repository (but the quality is not as good)

-----------------------------------
### III. Product metrics  

**INFO:**  

There is 3 datasets and 6 main metrics to calculate.

**1.OnTime metric**  

**Metric illustrates was the order delivered on planned date or not, %**

It is necessary to see the change of the metric by day;
Metric change needed by carriers, by type of delivery, by warehouse clusters, by timeslots

**2. PromisedClick2Delivery metric**  

**Metric of the promised delivery time (from the order date to the first planned delivery date), in days.**

Necessary metric changes by days, weeks, months
It is necessary to compare carriers
It is necessary to understand the level of metrics from cluster to cluster

**3. Click2Delivery metric**  

**Metric of the the actual delivery time (from the order date to the moment of delivery fact), in days.**

Comparison with the PromisedClick2Delivery metric is required
It is necessary to understand the clusters where orders are delivered faster than we were promised

**4. GMV Accepted**  

**Metric of the total sales volume (calculated for orders per day), in rubles.**

Necessary to see the change of the metric by day
Metric change needed by carriers, by type of delivery, by warehouse clusters

**5. Return Rate**  

**Metric of the orders return ratio (the ratio of the number of returns to orders on selected day), in %.**

Necessary to see the change of the metric daily
Metric change needed by carriers, by warehouse and customers clusters

**6. GMV D-R**  

**Metric that shows the total volume of delivered orders minus returns amount, rubles**  

Necessary to see the change of the metric by day (for all days that were earlier than the date of calculation of the metric)
Metric change needed by carriers, by type of delivery, by warehouse clusters  

-----------------
#### Datasets:

**logistics_online** dataset contains logistic information:  

*date_order* - date when the order was made by client  
*number_order* - unique order ID (ID can be repeated, because orders are rescheduled and have earlier delivery dates)  
*timeslot_number* - Ordering of the number of attempts to deliver on the planned delivery date.  
					If the delivery was not carried out, then the order is transferred to a new date
*result_data_order* - date when the order was delivered to the client  
*delivery_type* - the method chosen by the client to receive the order (courier or pickup)  
*carrier_name* - the carrier by which the order is delivered to the client  
*planned_data_order* - planned order date  
*seller_ID* - seller's unique ID  
*order_status* - the last actual order status (the last order status is fixed in all logs with the same ID)  
*customer_cluster* - region of customer  
*warehouse_cluster* - region of warehouse  


**product_online** dataset contains information about orders and products:

*number_order* - unique order ID (ID can be repeated, because orders are rescheduled and have earlier delivery dates)
*product_ID* - unique product ID (ID can be repeated, because one product can be in different orders of clients)
*product_price* - price of one unit of good in rubles
*amount* - amount of product units in order

**returns_online** dataset contains information about order returns:

*number_order* - unique order ID (ID can be repeated, because orders are rescheduled and have earlier delivery dates)
*product_ID* - unique product ID (ID can be repeated, because one product can be in different orders of clients)
*amount_return* - amount of product units which were returned (no more than the number of products purchased in the same order)
*return_date* - date of order return  


**HOW ITS DONE**  
All work is described and done in Jupyter Notebook. You can watch it in repo - "Product metrics_OZON.ipynb" file  
The datasets were uploaded by SQL queries connected to the Clickhouse.  
Further, all preprocessing, calculation of metrics and visualization in graphs was carried out in python.  

**Used libraries:** pandas, numpy, plotly, pandahouse, seaborn, matplotlib.
The way of work is described in details in the Notebook using the comments.

