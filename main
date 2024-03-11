<?php
// Replace these with your actual values
$base_url = "http://glpi.example.com/apirest.php"; // GLPI API base URL
$username = "your_username"; // Your GLPI username
$password = "your_password"; // Your GLPI password
$app_token = "your_application_token"; // Your GLPI application token
$session_token = ''; // Initialize the session token variable

// Make a GET request to GLPI API using the stored session token
function get_glpi_data($session_token, $ticket_id) {
    global $base_url, $app_token, $username, $password;

    $headers = array(
        "App-Token: $app_token",
        "Session-Token: $session_token",
    );

    $url = "$base_url/Ticket/$ticket_id?expand_dropdowns=true";
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $response = curl_exec($ch);
    curl_close($ch);

    return json_decode($response, true);
}

// Send data to Telegram channel
function send_to_telegram($data) {
    $bot_token = "your_bot_token";
    $chat_id = "your_chat_id";
    $name = $data["name"];
    $creation = $data["date_creation"];
    $category = $data["itilcategories_id"];
    $location = $data["locations_id"];
    $content = array_filter(explode("\n", strip_tags(htmlspecialchars_decode($data["content"]))));
    foreach ($content as $key =>$value) {$content_tmp[] = $value;}
    $content = array();
    foreach ($content_tmp as $key =>$value) {if ($key %2 == 0) {$content[$key]= $value . ":" . $content_tmp[$key+1];}}
    $content = implode("\n",$content);
    $message = "A new ticket has been created: $name\nat the $creation \nType : $category\nLocation: $location\n$content";
    $url = "https://api.telegram.org/bot$bot_token/sendMessage?chat_id=$chat_id&text=" . urlencode($message);
    file_get_contents($url);
}

// Function to perform a cURL GET request
function curl_get($url, $headers) {
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $response = curl_exec($ch);
    $response_code = curl_getinfo($ch, CURLINFO_HTTP_CODE); // Get HTTP response code
    curl_close($ch);
    return array('response' => $response, 'response_code' => $response_code);
}

// Function to perform a cURL POST request
function curl_post($url, $data, $headers) {
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $response = curl_exec($ch);
    $response_code = curl_getinfo($ch, CURLINFO_HTTP_CODE); // Get HTTP response code
    curl_close($ch);
    return array('response' => $response, 'response_code' => $response_code);
}

// Send a GET request to check if output is 400
$headers = array(
    "App-Token: $app_token",
    "Authorization: Basic " . base64_encode("$username:$password")
);

$get_response = curl_get($base_url . "/getMyProfiles", $headers);
$response = $get_response['response'];
$response_code = $get_response['response_code'];

if ($response == false) {
    echo "HTTP request failed.\n";
} else {
    if ($response_code == 400) {
        // If output is 400, initialize a session
        $session_data = array();
        $session_response = curl_post($base_url . "/initSession", $session_data, $headers);

        //echo "Session Response:", $session_response['response'], "\n";

        $session_code = $session_response['response_code'];
        if ($session_code == 200) {
            $session_data = json_decode($session_response['response'], true);
            $session_token = $session_data["session_token"];
            //echo "Session Token:", $session_token, "\n";

            // Rest of the code remains the same...
            // Replace this with your actual values
            $app_token = "your_application_token";
            $ticket_id_file = '/root/lastticket'; // Path to the ticket ID file
            // Read the starting ticket ID from the file
            $starting_ticket_id = (int) file_get_contents($ticket_id_file);
            $ticket_id = $starting_ticket_id + 1 ; // Start from the next ticket ID
            while (true) {
                $glpi_data = get_glpi_data($session_token, $ticket_id);
        	            if (!isset($glpi_data[0])) {
	            	        $starting_ticket_id = $ticket_id; // Update the starting ticket ID
                		    file_put_contents($ticket_id_file, $starting_ticket_id);
	        	            send_to_telegram($glpi_data);
				    $ticket_id++;
                           } else {
                	            break;
                                  }
                        }
        } else {
            echo "Failed to initialize session.\n";
        }
    } else {
        echo "Output is not 400.\n";
    }
}

//echo "Session Token as a variable:", $session_token, "\n";


?>


