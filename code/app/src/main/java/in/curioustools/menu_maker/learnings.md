1. Changing an entity file from java to kotlin will still require a version upgrade



2. data classes automatically provide with a hashcode and equals instance (maybe string too)
   (not sure if they really provide a decent logic for calculation of such functions) 
   STANDARD EQUALS AND HASHCODE FUNCTIONS:
   
```kotlin
    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (javaClass != other?.javaClass) return false

        other as MenuCategory

        if (id != other.id) return false
        if (name != other.name) return false

        return true
    }
    override fun hashCode(): Int {
        var result = id.hashCode()
        result = 31 * result + name.hashCode()
        return result
    }
```


3. 
insert query generated by room :  ``` "INSERT OR ABORT INTO `item_cat_relation` (`cat_id`,`item_id`) VALUES (?,?)" ```
delete query generated by room :  ``` "DELETE FROM menu_item WHERE item_id = ?"; ```
getall from a n:n relation table query by room:

```
Query1
"""
SELECT 
    `menu_item`.`item_id`   AS `item_id`,
    `menu_item`.`item_name` AS `item_name`,
    `menu_item`.`p_half`    AS `p_half`,
    `menu_item`.`p_full`    AS `p_full`,
    _junction.`cat_id` FROM `item_cat_relation` AS _junction
        INNER JOIN `menu_item` ON (_junction.`item_id` = `menu_item`.`item_id`)
            WHERE _junction.`cat_id` IN (" 
""" 
+__mapKeySet.size(); + ")"

Query2
"""
 _stringBuilder.append("SELECT `menu_item`.`item_id` AS `item_id`,`menu_item`.`item_name` AS `item_name`,`menu_item`.`p_half` AS `p_half`,`menu_item`.`p_full` AS `p_full`,_junction.`cat_id` FROM `item_cat_relation` AS _junction INNER JOIN `menu_item` ON (_junction.`item_id` = `menu_item`.`item_id`) WHERE _junction.`cat_id` IN (");

"""


``` 

create tables, db , indexes

```
 _db.execSQL("CREATE TABLE IF NOT EXISTS `menu_item` (`item_id` TEXT NOT NULL, `item_name` TEXT NOT NULL, `p_half` INTEGER NOT NULL, `p_full` INTEGER NOT NULL, PRIMARY KEY(`item_id`))");
        _db.execSQL("CREATE TABLE IF NOT EXISTS `item_cat_relation` (`cat_id` TEXT NOT NULL, `item_id` TEXT NOT NULL, PRIMARY KEY(`cat_id`, `item_id`))");
        _db.execSQL("CREATE INDEX IF NOT EXISTS `index_item_cat_relation_cat_id` ON `item_cat_relation` (`cat_id`)");
        _db.execSQL("CREATE INDEX IF NOT EXISTS `index_item_cat_relation_item_id` ON `item_cat_relation` (`item_id`)");
        _db.execSQL("CREATE TABLE IF NOT EXISTS `menu_cat` (`cat_id` TEXT NOT NULL, `cat_name` TEXT NOT NULL, PRIMARY KEY(`cat_id`))");
        _db.execSQL("CREATE TABLE IF NOT EXISTS room_master_table (id INTEGER PRIMARY KEY,identity_hash TEXT)");
        _db.execSQL("INSERT OR REPLACE INTO room_master_table (id,identity_hash) VALUES(42, '86b798f5639ef006635bed47cac1e148')");
      }

```