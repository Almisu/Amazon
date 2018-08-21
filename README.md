# Amazon
Make $34 to $200 a day with Kindle E-Book
<?php
/**
 * This is an example of Postback file for saving conversions into database.
 * Do not forget to change values for database connection.
 */
// Define your password, set to blank "" for no password. (optional).
$your_postback_password = "";
// Your database credentials.
define("MYSQL_HOST", "localhost");
define("MYSQL_DB", "your_db");
define("MYSQL_TABLE", "cpalead_postback");
define("MYSQL_USER", "username");
define("MYSQL_PASS", "password");
// Connecting to database, using MySqli
$mysqli = new mysqli(MYSQL_HOST, MYSQL_USER, MYSQL_PASS, MYSQL_DB);
if ($mysqli->connect_errno) {
    echo "Failed to connect to MySQL: (" . $mysqli->connect_errno . ") " . $mysqli->connect_error;
}
// Setup postback variables
$password       = $_REQUEST['password'];
$subid          = $_REQUEST['subid'];
$campaign_id    = $_REQUEST['campaign_id'];
$campaign_name  = $_REQUEST['campaign_name'];
$subid2         = $_REQUEST['subid2'];
$subid3         = $_REQUEST['subid3'];
$payout         = $_REQUEST['payout'];
$ip_address     = $_REQUEST['ip_address'];
$gateway_id     = $_REQUEST['gateway_id'];
$lead_id        = $_REQUEST['lead_id'];
$country_iso    = $_REQUEST['country_iso'];
$points         = $_REQUEST['virtual_currency'];
$time           = time();
// If (optional) password is set, deny access.
if (!empty($your_postback_password) && isset($password) && !empty($password) && ($your_postback_password != $password)) {
  exit;
}
// Insert log into database
if (!($stmt = $mysqli->prepare("INSERT INTO ".MYSQL_DB.".".MYSQL_TABLE." ('subid', 'campaign_id', 'campaign_name', 'subid2', 'subid3', 'payout', 'ip_address', 'gateway_id', 'lead_id', 'country_iso', 'points', 'timestamp') VALUES ((?), (?), (?), (?), (?), (?), (?), (?), (?), (?), (?), (?))"))) {
  echo "Preparation failed: (" . $mysqli->errno . ") " . $mysqli->error;
}
$stmt->bind_param('sisssdsiisis', $subid, $campaign_id, $campaign_name, $subid2, $subid3, $payout, $ip_address, $gateway_id, $lead_id, $country_iso, $points, $time);
if (!$stmt->execute()) {
  echo "Execution failed: (" . $stmt->errno . ") " . $stmt->error;
} else {
  printf("Added new conversion with subid: ".$subid." .\n");
}
?>
