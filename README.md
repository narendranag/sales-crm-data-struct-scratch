# sales-crm-data-struct-scratch
Trying to figure out what the data structure should look like for a CRM that meets our specific needs. Completely experimental

Let’s break down the entities and relationships — this will serve as a reference for building out the data model in an ORM engine.

### Entities and Relationships:

---

### **1. Salesperson**
#### Attributes:
- `id` (int): Unique identifier for the salesperson.
- `name` (string): Full name of the salesperson.
- `email` (string): Contact email of the salesperson (must be unique).
- `phone` (string): Contact phone number of the salesperson.
- `target` (float): The sales target for the salesperson.
- `stretch_target` (float): The stretch sales target for the salesperson.
- `team_id` (int): Foreign key linking to the **Team** the salesperson belongs to.
- `level_id` (int): Foreign key linking to the **Level** the salesperson is at.

#### Relationships:
- **Team**: A salesperson belongs to one **Team** (Many-to-One).
- **Level**: A salesperson belongs to one **Level** (Many-to-One).
- **Deals**: A salesperson can have multiple **Deals** (One-to-Many).
- **Reports**: A salesperson can have multiple **Reports** (One-to-Many).

---

### **2. Team**
#### Attributes:
- `id` (int): Unique identifier for the team.
- `name` (string): Name of the team.
- `leader_id` (int): Foreign key linking to the **Salesperson** who leads the team.

#### Relationships:
- **Salespersons**: A team can have multiple **Salespersons** (One-to-Many).
- **Leader**: The team is led by one **Salesperson** (One-to-One).
- **Commission Structure**: Each team has one **Commission Structure** (One-to-One).

---

### **3. Level**
#### Attributes:
- `id` (int): Unique identifier for the level.
- `name` (string): Name or title of the level (e.g., "Junior", "Senior", etc.).
- `tier_commission_rate` (float): The commission rate associated with this level.

#### Relationships:
- **Salespersons**: A level can be associated with multiple **Salespersons** (One-to-Many).

---

### **4. Client**
#### Attributes:
- `id` (int): Unique identifier for the client.
- `name` (string): Client name or company name.
- `industry` (string): The industry the client operates in (e.g., "Retail", "Automotive").
- `email` (string): Client's contact email.
- `phone` (string): Client's contact phone number.
- `contact_person` (string): Main point of contact at the client.
- `contract_value` (float): The value of the contract associated with the client.

#### Relationships:
- **Deals**: A client can have multiple **Deals** (One-to-Many).
- **Media Campaigns**: A client can have multiple **Media Campaigns** (One-to-Many).
- **Reports**: A client can have multiple **Reports** (One-to-Many).

---

### **5. Media Campaign**
#### Attributes:
- `id` (int): Unique identifier for the media campaign.
- `client_id` (int): Foreign key linking to the **Client** the campaign belongs to.
- `name` (string): Name of the media campaign.
- `start_date` (date): Start date of the campaign.
- `end_date` (date): End date of the campaign.
- `budget` (float): The budget allocated for the campaign.
- `status` (enum): The status of the campaign (e.g., "ongoing", "completed", "pending").

#### Relationships:
- **Client**: A media campaign belongs to one **Client** (Many-to-One).
- **Ad Inventories**: A media campaign can have multiple **Ad Inventories** (One-to-Many).

---

### **6. Ad Inventory**
#### Attributes:
- `id` (int): Unique identifier for the ad inventory.
- `type` (string): Type of ad inventory (e.g., "video", "banner", "native").
- `location` (string): Location where the ad will be displayed (e.g., "homepage", "sidebar").
- `duration` (float): Duration (in seconds or minutes) of the ad inventory.
- `price` (float): Price of the ad inventory.
- `available` (boolean): Whether the ad inventory is available for sale.
- `campaign_id` (int): Foreign key linking to the **Media Campaign**.

#### Relationships:
- **Media Campaign**: Ad inventory belongs to one **Media Campaign** (Many-to-One).
- **Deals**: Ad inventory can be part of multiple **Deals** (One-to-Many).

---

### **7. Deal**
#### Attributes:
- `id` (int): Unique identifier for the deal.
- `client_id` (int): Foreign key linking to the **Client** involved in the deal.
- `salesperson_id` (int): Foreign key linking to the **Salesperson** handling the deal.
- `ad_inventory_id` (int): Foreign key linking to the **Ad Inventory** being sold.
- `amount` (float): The total amount of the deal.
- `contract_date` (date): The date the deal was signed.
- `status` (enum): The current status of the deal (e.g., "open", "closed", "pending").

#### Relationships:
- **Client**: A deal belongs to one **Client** (Many-to-One).
- **Salesperson**: A deal involves one **Salesperson** (Many-to-One).
- **Ad Inventory**: A deal involves one **Ad Inventory** (Many-to-One).

---

### **8. Commission Structure**
#### Attributes:
- `id` (int): Unique identifier for the commission structure.
- `base_rate` (float): The base commission rate.
- `tiered_rates` (string/JSON): A serialized list or JSON field storing tiered commission rates.
- `team_target` (float): The team-wide sales target.
- `team_stretch_target` (float): The stretch target for the team.
- `team_id` (int): Foreign key linking to the **Team** the commission structure applies to.

#### Relationships:
- **Team**: A commission structure applies to one **Team** (One-to-One).

---

### **9. Report**
#### Attributes:
- `id` (int): Unique identifier for the report.
- `salesperson_id` (int): Foreign key linking to the **Salesperson** for whom the report is generated.
- `client_id` (int): Foreign key linking to the **Client** the report is about.
- `campaign_performance` (float): Performance metric of the campaign.
- `revenue_generated` (float): Total revenue generated from the campaign.
- `commission_earned` (float): Total commission earned by the salesperson for the campaign.

#### Relationships:
- **Salesperson**: A report is linked to one **Salesperson** (Many-to-One).
- **Client**: A report is linked to one **Client** (Many-to-One).

---

### Relationships Summary:

- **Salesperson** belongs to one **Team** and one **Level** but can have many **Deals** and **Reports**.
- **Team** has many **Salespersons** and one **Leader**, and each team has a unique **Commission Structure**.
- **Level** can have many **Salespersons**.
- **Client** can have many **Deals**, **Media Campaigns**, and **Reports**.
- **Media Campaign** belongs to one **Client** and can have many **Ad Inventories**.
- **Ad Inventory** belongs to one **Media Campaign** but can be part of many **Deals**.
- **Deal** involves one **Client**, one **Salesperson**, and one **Ad Inventory**.
- **Commission Structure** applies to one **Team**.
- **Report** is linked to one **Salesperson** and one **Client**.

This breakdown provides a solid foundation for how the data should be structured and related in your media sales CRM. You can use this as a blueprint to implement the data model in your chosen ORM engine. Let me know if you'd like further details or specific implementation examples!