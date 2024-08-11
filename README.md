DOCUMENTATION API

------------------------------------------------API Routes
------------Public Routes
POST /api/login - Authenticates a user and returns a token.
------------Protected Routes (requires auth:sanctum middleware)
POST /api/logout - Logs out the authenticated user.
----Client Routes
GET /api/client - Retrieves a list of all clients.
POST /api/client/add - Adds a new client.
PUT /api/client/update/{id} - Updates an existing client by ID.
DELETE /api/client/remove/{id} - Deletes a client by ID.
---Matière Première Routes
GET /api/matiere - Retrieves a list of all matières premières.
GET /api/matiere/{id} - Retrieves a single matière première by ID.
POST /api/matiere/add - Adds a new matière première.
PUT /api/matiere/update/{id} - Updates an existing matière première by ID.
DELETE /api/matiere/remove/{id} - Deletes a matière première by ID.
---Mouvement Stock Routes
GET /api/movement - Retrieves a list of all stock movements.
---Produit Routes
GET /api/produit - Retrieves a list of all produits finis.
GET /api/produit/{id} - Retrieves a single produit fini by ID.
POST /api/produit/add - Adds a new produit fini.
PUT /api/produit/update/{id} - Updates an existing produit fini by ID.
DELETE /api/produit/remove/{id} - Deletes a produit fini by ID.
---Commande Routes
GET /api/commande - Retrieves a list of all commandes.
POST /api/commande/add - Adds a new commande.
PUT /api/commande/update/{id} - Updates an existing commande by ID.
DELETE /api/commande/remove/{id} - Deletes a commande by ID.
-----------------------Controller Methods
---ClientController
index()
Description: Retrieves a list of all clients.
Returns: JSON array of all client records.
store(Request $request)
Description: Validates and stores a new client.
Parameters:
name (required): Client's name.
address (required): Client's address.
phone (required): Client's phone number.
email (required): Client's email (must be unique).
Returns: JSON object of the newly created client or validation errors.
show($id)
Description: Retrieves a single client by ID.
Parameters:
id (required): ID of the client.
Returns: JSON object of the client.
update(Request $request, $id)
Description: Updates an existing client.
Parameters:
id (required): ID of the client.
Request fields for name, address, phone, and email are optional but will be validated if provided.
Returns: JSON object of the updated client or validation errors.
destroy($id)
Description: Deletes a client by ID.
Parameters:
id (required): ID of the client.
Returns: 204 No Content on successful deletion.
----MatierePremiereController
index()
Description: Retrieves a list of all matières premières.
Returns: JSON array of all matières premières.
store(Request $request)
Description: Validates and stores a new matière première.
Parameters:
name (required): Name of the matière première.
quantity (required): Quantity of the matière première.
unit (required): Unit of measurement.
priceU (required): Price per unit.
Returns: JSON object of the newly created matière première or validation errors.
show($id)
Description: Retrieves a single matière première by ID.
Parameters:
id (required): ID of the matière première.
Returns: JSON object of the matière première.
update(Request $request, $id)
Description: Updates an existing matière première.
Parameters:
id (required): ID of the matière première.
Request fields for name, quantity, unit, and priceU are optional but will be validated if provided.
Returns: JSON object of the updated matière première or validation errors.
destroy($id)
Description: Deletes a matière première by ID and logs the movement in stock.
Parameters:
id (required): ID of the matière première.
Returns: 204 No Content on successful deletion.
---produitController
index()
Description: Retrieves a list of all produits finis.
Returns: JSON array of all produits finis.
store(Request $request)
Description: Validates and stores a new produit fini, updating stock accordingly.
Parameters:
name (required): Name of the produit fini.
quantity (required): Quantity of the produit fini.
unit (required): Unit of measurement.
priceU (required): Price per unit.
matiere_premiere_quantities (required): Array of matières premières used in the product.
Each entry should have an id (matière première ID) and quantity.
Returns: JSON object of the newly created produit fini or validation errors.
show($id)
Description: Retrieves a single produit fini by ID.
Parameters:
id (required): ID of the produit fini.
Returns: JSON object of the produit fini.
update(Request $request, $id)
Description: Updates an existing produit fini and adjusts stock for any associated matières premières.
Parameters:
id (required): ID of the produit fini.
Request fields for name, quantity, unit, priceU, and matiere_premiere_quantities are optional but will be validated if provided.
Returns: JSON object of the updated produit fini or validation errors.
destroy($id)
Description: Deletes a produit fini by ID, restores stock quantities for associated matières premières, and logs the stock movement.
Parameters:
id (required): ID of the produit fini.
Returns: 204 No Content on successful deletion.
---CommandeController
index()
Description: Retrieves a list of all commandes.
Returns: JSON array of all commandes.
store(Request $request)
Description: Validates and stores a new commande, adjusting stock for associated produits finis.
Parameters:
client_id (required): ID of the client placing the order.
commandeID (required): Unique ID for the commande.
dateCommande (required): Date of the commande.
dateLivraison (optional): Date of livraison (if applicable).
status (required): Status of the commande.
produits (required): Array of produits finis in the commande.
Each entry should have an id (produit fini ID) and quantity.
Returns: JSON object of the newly created commande or validation errors.
show($id)
Description: Retrieves a single commande by ID.
Parameters:
id (required): ID of the commande.
Returns: JSON object of the commande.
update(Request $request, $id)
Description: Updates an existing commande and adjusts stock for any associated produits finis.
Parameters:
id (required): ID of the commande.
Request fields for client_id, commandeID, dateCommande, dateLivraison, status, and produits are optional but will be validated if provided.
Returns: JSON object of the updated commande or validation errors.
destroy($id)
Description: Deletes a commande by ID, restores stock quantities for associated produits finis, and logs the stock movement.
Parameters:
id (required): ID of the commande.
Returns: 204 No Content on successful deletion.
--------------------Models
---Client
Represents a client in the system.
Fields:
id: Unique identifier.
name: Name of the client.
address: Address of the client.
phone: Phone number of the client.
email: Email of the client.
---MatierePremiere
Represents a raw material (matière première) in the system.
Fields:
id: Unique identifier.
name: Name of the matière première.
quantity: Quantity of the matière première.
unit: Unit of measurement.
priceU: Price per unit.
---ProduitFini
Represents a finished product (produit fini) in the system.
Fields:
id: Unique identifier.
name: Name of the produit fini.
quantity: Quantity of the produit fini.
unit: Unit of measurement.
priceU: Price per unit.
matiere_premiere_quantities: Array of matières premières used in the product.
---Commande
Represents an order (commande) in the system.
Fields:
id: Unique identifier.
client_id: ID of the client placing the order.
commandeID: Unique ID for the commande.
dateCommande: Date of the commande.
dateLivraison: Date of livraison (if applicable).
status: Status of the commande.
produits: Array of produits finis in the commande.
