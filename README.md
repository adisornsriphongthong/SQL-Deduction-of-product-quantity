# SQL-Deduction-of-product-quantity

DROP PROCEDURE IF EXISTS createOrder;

DELIMITER //

CREATE PROCEDURE createOrder(IN user_id INT, IN product_id INT, IN product_quantity INT)
BEGIN

    UPDATE products
    SET quantity = quantity - product_quantity
    WHERE Id = product_id;
    
    SELECT quantity INTO @quantity
    FROM products
    WHERE Id = product_id;

    IF @quantity < 0 THEN 
       SIGNAL SQLSTATE '45000'
       SET MESSAGE_TEXT = 'Product quantity cannot be less than 0';
    END IF;
    
    SELECT * FROM products
    WHERE Id = product_id;
    
END//

DELIMITER ;

CALL createOrder(1, 1, 1);
