# Supply chain fundamentals
#### What is a supply chain?
The job of a supply chain is to get the right product, to the right customer, at the right time, at the right place, in the right quantity, in the right condition, and at the right cost
#### Two kinds of supply chain
* Traditional: raw material → purchasing → product → distribution → customers
* Complex: supplier → manufacturers → distributors → retailers → customers
#### What is a demand?
The need for a particular item, product, component, or service
#### Two types of demand
* Independent: can be forecasted, usually come from customer demands (but not always)
* Dependent: calculated from independent demands (cannot be forecasted)
#### Demand patterns
* Trend: demand is changing in a steady rate from period to period
* Random fluctuation: causes by random occurrence, vary around the average and the variation will cancel each other out. Medical is in this category 
* Seasonal fluctuation: demands are known to repeat themselves each year, it takes short time periods, such as monthly or weekly
* Cyclical: has wave-like fluctuation, caused by external influence, the period is more than one year
#### Forecasting demand
Review and understand historical demand pattern, fluctuation, variation
* Simple moving average:
  * use previous N months demand to predict the demand of this month, usually use N = 3 ~ 6
    * For example, if N = 3, then the demand of X(t=4) = [X(t=1) + X(t=2) + X(t=3)]/3 
  * remember to round down.
* Weighted moving average:
  * The weights Ds are decided by supply chain expert, for example
    * D1 = 75%, 70%, 80% most recent demand
    * D2 = 25%, 20%, 15% 2nd recent demand
    * D3 = 5%, 10%, 5% 3rd recent demand
  * For example, if N=3, then X(t=4) = (D1 * X(t=3) + D2 * X(t=2) + D3 * X(t=1)]/3 
* Exponential smoothing:
  * Most used
  * Weighting factor: 0 < alpha < 1, the value of alpha is decided by data
  * X(t) = alpha * X(t-1, true value) + (1 - alpha) X(t-1, prediction)
#### Almost all forecasts will be wrong!
* Need to minimize forecast error
  * Forecast error = the error between actual demand and forecasted demand
#### 4 ways to measure forecast error
* Cumulative sum of error: $\sum (y_{true} - \hat{y}_{pred})$
  * Step1: calculate demand - forecasted for each period
    * positive value: demand > forecasted → need more products
    * negative value: demand < forecasted → need less products
  * Step2: sum results in step 1
* Mean square error: $\sqrt{\frac{\sum (y_{true} - \hat{y}_{pred}}{N}}$
  * Step1: calculate (demand - forecasted)^2
  * Step2: sum results in step1
  * Step3: calculate the average in step2
  * Step4: take square root of step3
* Mean absolute deviation: $\frac{\sum |y_{true} - \hat{y}_{pred}|}{N}$
  * Step1: calculate |demand - forecasted|
  * Step2: sum results in step1
  * Step3: calculate the average in step2
* Mean absolute percent error: $\frac{1}{N}\sum\frac{absolute deviation}{y_{true}} \times 100$
  * Step1: mean absolute deviation / demand * 100%
  * Step2: sum results in step2
  * Step3: calculate the average in step2
#### What is manufacturing?
* Transforms raw materials into finished goods that meet the needs of customers
  * Raw material → product
* The main goal of manufacturing is to create value for producer and customers
  * Value: worth of good or service or what customers will to pay for it
#### Manufacturing environment
* A kind of strategy
* ETO: Engineer-to-Order
  * has longest lead time
* MTO: Manufacture-to-Order
  * starts to manufacture after receiving customer order
* ATO: Assemble-to-Order
  * parts are ready in the inventory, according to customer's configuration to assemble parts to form the final product
* MTS: Manufacture-to-Stock
  * products are manufactured to forecast
  * shortest lead time
  * products already in stock and ready to ship
  * products are held in the inventory until the customer order is received
* Mass customization
#### Product life cycle
* From the product introduce to the market to the time disappear from the market
* Bell shape curve
* 4 stages in product life cycle, different stage use different strategy
  * Introduction stage
    * Ad, promotion, price is high
    * ETO
  * Growth stage
    * production increases and price decreases
    * competition begin
    * MTO
  * Maturity state
    * Product well establishes in the market
    * MTS
  * Decline stage
    * Sales decreases, profit decreases
    * Introduce variation of product to stimulate the demand
    * MTO
  * Phase-out stage
    * Disappear from the market






# Supply chain metrics
#### Inventory Velocity = (opening stock / next months's sales forecast)
#### Perfect order measurement = \[(total orders - error orders) / total orders\] x 100
* Calculate the **error-free** rate of each stage of a purchase order
  * error in order forecasting for procurement
  * error in warehouse pickup process
  * error in invoicing
  * error in shipping orders
#### Inventory turnover ratio (ITR) = cost of goods / ((opening stock - closing stock)/2)
#### Cash to cash cycle time = materials payment date - customer order payment date
* The number of days between paying for materials and getting paid for product
* Take averaged (weekly, monthly, quarterly)
* Many materials → weighted average material payment date
* A fast cash to cash indicates a lean and profitable supply chain
#### Days of supply = (average inventory on hand (as value) / average monthly demand (as value)) x 30 (for measurement on monthly basis)
* The number of days it would take to run out of supply if it was not replenished
* Minimize this metric to reduce the risks of excess and obsolete inventory
#### Days sales outstanding = (receivables / sales) x days in period
#### Inventory turnover = (cost of goods / average inventory)
#### Customer order cycle time = (actual delivery date - purchase order creation date)
* How long it takes to deliver a customer order after purchase order (PO) is received
* Variation: **requested delivery date - purchase order creation date**
#### Gross margin return on investment (GMROI) = gross profit / \[(open stock - closing stock) / 2\] x 100
#### Turn-earn index (TEI) = ITR x (gross profit in %) x 100
#### On time shipping rate = (number of on time items / total items) x 100
#### Fill rate = (1 - \[(total items - shipped items) / total items\]) x 100
* The percentage of a customer's order that is filled on the first shipment
* Has implications for transportation efficiency
#### Average payment period for production materials = (materials payable / total cost of materials) x days in period
#### Supply chain cycle time = sum of the longest lead times for each stage of the cycle
* The time it would take to fill a customer order if inventory levels were zero
* Overall efficiency of the supply chain
* Short cycles make for a more efficient and agile supply chain
* Inventory days of supply = (Inventory on hand / average daily usage)
#### Freight cost per unit = total freight cost / number of items
#### Freight bill accuracy = (error-free freight bills / total freight bills) x 100
