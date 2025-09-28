## Diagramme de classes
@startuml
package "OrderCustomPC" {

class StoreKeeper extends StockManager {
    + type: String
    + subType: String
    + description: String
    + quantity: int
    + comment: String
    + creationTime: DateTime
    + modificationTime: DateTime
    + addStock(): void
    + updateStock(): void
}

class Requester {
    + uniqueOrderID: String
    + uniqueStockID: String
    + creationTime: DateTime
    + modificationTime: DateTime
    + requestStock(): void
    + cancelOrder(): void
}

class Assembler {
    + getOrderList(): List<Order>
    + getStockList(): List<Stock>
    + rejectOrder(): void
    + approveOrder(): void
}
class WelcomePage {
    + displayConnectionMessage(): void
    + verifyUserInformation(String email, String password): void
}


class Administrator {
    + creationTime: DateTime (auto-generated)
    + modificationTime: DateTime (auto-generated)
    + addUser(): void
    + modifyUser(): void
    + deleteUser(): void
    + authenticate(): Boolean
}

Requester "*" <-- "1" StoreKeeper : Relies on stock
Requester "*" --> "1" Assembler : Submits order to
Requester "*" ..> "1" Administrator : Depends on
StoreKeeper "1" --> "1" Assembler : Supplies components to
StoreKeeper "1" --> "*" Requester : Maintains stock for
StoreKeeper "1" ..> "1" Administrator : Depends on
Assembler "1" ..> "1" Administrator : Depends on
Administrator "1" ..> "*" Requester : Manages
StockManager "1" --> "*" Component : Manages

class DatabaseInitializer {
    + initializeFromFile(File file); void
    + addUserToDatabase(String email, String password, String firstName, String lastName): void
}

Administrator "1" --> "1" DatabaseInitializer : Uses
Administrator "1" --> "1" DatabaseHelper : Uses

class DatabaseHelper {
    - static final String DATABASE_NAME = "pc_management.db"
    - static final int DATABASE_VERSION = 2
    - static final String TABLE_USERS = "users"
    - static final String COLUMN_ID = "id"
    - static final String COLUMN_ROLE = "role"
    - static final String COLUMN_USERNAME = "username"
    - static final String COLUMN_PASSWORD = "password"
    - static final String TABLE_STOCK = "stock"
    - static final String COLUMN_STOCK_ID = "stock_id"
    - static final String COLUMN_TYPE = "type"
    - static final String COLUMN_SUBTYPE = "subtype"
    - static final String COLUMN_TITLE = "title"
    - static final String COLUMN_QUANTITY = "quantity"
    - static final String COLUMN_COMMENT = "comment"
    - static final String COLUMN_CREATION_TIME = "creation_time"
    - static final String COLUMN_MODIFICATION_TIME = "modification_time"
    - static final String TABLE_ORDERS = "orders"
    - static final String COLUMN_ORDER_ID = "order_id"
    - static final String COLUMN_STATUS = "status"
    - static final String COLUMN_REQUESTER_ID = "requester_id"
    - static final String COLUMN_ORDER_DATE = "order_date"
    - static final String TABLE_ORDER_DETAILS = "order_details"
    - static final String COLUMN_ORDER_DETAIL_ID = "order_detail_id"
    - static final String COLUMN_COMPONENT_ID = "component_id"
    - static final String COLUMN_QUANTITY_ORDERED = "quantity"

    + DatabaseHelper(Context context)
    + onCreate(SQLiteDatabase db)
    + onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
    + insertDefaultUsers(SQLiteDatabase db)
    + addUserInternal(SQLiteDatabase db, String role, String username, String password)
    + addUser(String role, String username, String password) : boolean
    + deleteUser(String username) : boolean
    + updateUser(String username, String newPassword) : boolean
    + initializeDatabase()
    + clearDatabase()
    + resetDatabase()
    + resetStock()
    + authenticateUser(String username, String password) : boolean
    + getUserRole(String username) : String
    + getPendingOrders() : Cursor
    + getAcceptedOrders() : Cursor
    + getShippedOrders() : Cursor
    + isStockAvailable(String orderId) : boolean
    + acceptOrder(String orderId) : boolean
    + addOrder(String caseType, String motherboard, String ram, int ramQty, String hardDrive, int hardDriveQty,
               String keyboardMouse, String monitor, int monitorQty, String web, String officeSuite, int officeSuiteQty,
               String devTools, int devToolsQty, String requesterId) : boolean
    + getComponentId(String componentTitle) : int
    + getCurrentDate() : String
    + rejectOrder(String orderId) : boolean
    + assembleAndShipOrder(String orderId) : boolean
    + addStockItem(String type, String subType, String title, int quantity, String comment, String creationTime, String modificationTime) : boolean
    + updateStockItem(String title, Integer quantity, String comment, String modificationTime) : boolean
    + deleteStockItem(String title) : boolean
    + getAllStockItems() : List<String>
    + addOrderAndGetId(String caseType, String motherboard, String ram, int ramQty, String hardDrive, int hardDriveQty,
                        String keyboardMouse, String monitor, int monitorQty, String web, String officeSuite, int officeSuiteQty,
                        String devTools, int devToolsQty, String requesterId, String orderDate, String status) : long
    + addOrderDetails(long orderId, int componentId, int quantity) : boolean

}

class Component {
    + getType(): String
    + getSubType(): String
    + getTitle(): String
    + getQuanity(): int
    + getComment(): String
    + getCreationDateTime(): LocalDateTime
    + getModificationDateTime(): LocalDateTime
    + setQuantity(int quantity): void
    + setComment(String comment): void
    + updateModificationDateTime(): void
}

class StockManager {
    + addComponent(String title, String type, String subType, int quantity, LocalDateTime creationDateTime, LocalDateTime modificationDateTime): void
    + updateComponent(String title, Integer newQuantity, String newComment, LocalDateTime modificationDateTime): void
    + removeComponent(String title): void
    + viewComponent(String title): Component
    + findComponentByTitle(String title): 
}

Administrator "1" <- "1" WelcomePage : Redirects to 
StoreKeeper "1" <- "1" WelcomePage : Redirects to 
Requester "1" <- "1" WelcomePage : Redirects to 
Assembler "1" <- "1" WelcomePage : Redirects to 
Administrator "1" -> "1" WelcomePage : Log out
StoreKeeper "1" -> "1" WelcomePage : Log out
Requester "1" -> "1" WelcomePage : Log out
Assembler "1" -> "1" WelcomePage : Log out


@enduml
}

## Diagrame d'état
@startuml
[*] -> Begin
Begin --> Running : Succeeded
Begin --> [*] : Aborted
state Running {
  Welcome : Enter log in information and log in
  Welcome --> Welcome : User interaction
  Welcome --> RolePage : Sign into respective role
  RolePage --> RolePage : User Interaction (Managing stock/orders, etc)
  RolePage --> Welcome : Log out
  Welcome --> Quit : Quit app
}
Quit --> [*] : Aborted
@enduml

## Diagramme de séquences
@startuml
actor Assembler
actor Requester
entity "Order System" as OrderSystem
entity "Stock Management" as Stock
entity "PC Assembly" as Assembly

Assembler -> OrderSystem : Check pending orders
OrderSystem -> OrderSystem : Retrieve list of pending orders
Assembler -> Stock : Validate stock availability
Stock -> Stock : Check stock levels
alt Stock sufficient
    Assembler -> OrderSystem : Approve order
    OrderSystem -> Assembly : Assemble PC
    Assembly -> OrderSystem : Return assembled PC
else Stock insufficient
    Assembler -> OrderSystem : Put order on hold
end
Assembler -> OrderSystem : Reject order (if applicable)
Requester -> OrderSystem : Create order
OrderSystem -> Requester : Confirm order created
Requester -> OrderSystem : View orders
OrderSystem -> Requester : Show order details
Requester -> OrderSystem : Delete order (under conditions)
OrderSystem -> Requester : Confirm order deleted

@enduml


## Diagramme d'activitées
@startuml
title User Login Activity Diagram

start

:User opens login page;
:User enters username and password;

if (Is login valid?) then (yes)
  :Display welcome message;
  if (User role?) then (Admin/StoreKeeper/Assmebler)
    :Redirect to Admin/StoreKeeper/Assembler Page;
  else (Regular User) 
    :Redirect to User Page;
  endif

else (no)
  :Display error message;
endif

:End of process;

stop
@enduml
