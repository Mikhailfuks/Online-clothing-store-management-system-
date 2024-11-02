CREATE TABLE Products (
    ProductId INT PRIMARY KEY IDENTITY(1,1),
    Name VARCHAR(255) NOT NULL,
    Description TEXT,
    Category VARCHAR(50) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    ImageURL VARCHAR(255),
    QuantityInStock INT NOT NULL,
    Size VARCHAR(10),
    Color VARCHAR(50)
);

-- Таблица категорий
CREATE TABLE Categories (
    CategoryId INT PRIMARY KEY IDENTITY(1,1),
    CategoryName VARCHAR(50) NOT NULL
);

-- Таблица пользователей
CREATE TABLE Users (
    UserId INT PRIMARY KEY IDENTITY(1,1),
    Username VARCHAR(50) NOT NULL,
    Password VARCHAR(255) NOT NULL,
    Email VARCHAR(255) NOT NULL,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    Role VARCHAR(50) -- Например, "Customer", "Admin"
);

-- Таблица заказов
CREATE TABLE Orders (
    OrderId INT PRIMARY KEY IDENTITY(1,1),
    UserId INT NOT NULL,
    OrderDate DATE NOT NULL,
    OrderStatus VARCHAR(50) NOT NULL, -- Например, "Processing", "Shipped", "Delivered"
    ShippingAddress VARCHAR(255),
    BillingAddress VARCHAR(255),
    TotalAmount DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (UserId) REFERENCES Users(UserId)
);

-- Таблица деталей заказа
CREATE TABLE OrderDetails (
    OrderDetailId INT PRIMARY KEY IDENTITY(1,1),
    OrderId INT NOT NULL,
    ProductId INT NOT NULL,
    Quantity INT NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (OrderId) REFERENCES Orders(OrderId),
    FOREIGN KEY (ProductId) REFERENCES Products(ProductId)
);

-- Таблица промокодов
CREATE TABLE PromoCodes (
    PromoCodeId INT PRIMARY KEY IDENTITY(1,1),
    Code VARCHAR(50) NOT NULL,
    DiscountAmount DECIMAL(10,2),
    DiscountType VARCHAR(50), -- Например, "Percentage", "Fixed"
    ExpirationDate DATE,
    UsageLimit INT,
    UsedCount INT
);

-- Таблица корзины
CREATE TABLE Carts (
    CartId INT PRIMARY KEY IDENTITY(1,1),
    UserId INT NOT NULL,
    FOREIGN KEY (UserId) REFERENCES Users(UserId)
);

-- Таблица товаров в корзине
CREATE TABLE CartItems (
    CartItemId INT PRIMARY KEY IDENTITY(1,1),
    CartId INT NOT NULL,
    ProductId INT NOT NULL,
    Quantity INT NOT NULL,
    FOREIGN KEY (CartId) REFERENCES Carts(CartId),
    FOREIGN KEY (ProductId) REFERENCES Products(ProductId)
);

-- Пример добавления категории
INSERT INTO Categories (CategoryName) VALUES ('Женская одежда'), ('Мужская одежда'), ('Детская одежда');

-- Пример добавления товара
INSERT INTO Products (Name, Description, Category, Price, ImageURL, QuantityInStock, Size, Color) VALUES
('Платье летнее', 'Лёгкое платье из хлопка', 'Женская одежда', 50.00, 'images/dress1.jpg', 10, 'M', 'Синий'),
('Футболка мужская', 'Хлопковая футболка с коротким рукавом', 'Мужская одежда', 20.00, 'images/tshirt1.jpg', 20, 'L', 'Чёрный');

-- Пример добавления пользователя
INSERT INTO Users (Username, Password, Email, FirstName, LastName, Role) VALUES
('john.doe', 'password123', 'john.doe@email.com', 'John', 'Doe', 'Customer');

-- Пример создания заказа
INSERT INTO Orders (UserId, OrderDate, OrderStatus, ShippingAddress, BillingAddress, TotalAmount) VALUES
(1, '2023-03-15', 'Processing', 'ул. Ленина, д. 1', 'ул. Ленина, д. 1', 70.00);

-- Пример добавления деталей заказа
INSERT INTO OrderDetails (OrderId, ProductId, Quantity, Price) VALUES
(1, 1, 1, 50.00),
(1, 2, 1, 20.00);
