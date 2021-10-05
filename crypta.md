```
/**
 * Cript And Decript baseado em AES-256-CBC 
 * 
 * @param string $string String a ser encroptada ou decriptada
 * @param string $action String tipo da ação esperada
 * @return string String tratada pelo processo de crypta
 * @author lukbarros
 */
function encriptarDecriptar($string, $action = 'encrypt')
{
    $encrypt_method = "AES-256-CBC";
    $secret_key = env('CHECKOUT_REDIRECT_SALT', NULL); 
    $secret_iv =  md5(env('CHECKOUT_REDIRECT_SALT', NULL)); 
    $key = hash('sha256', $secret_key);
    $iv = substr(hash('sha256', $secret_iv), 0, 16);
    if ($action == 'encrypt') 
    {
        $output = openssl_encrypt($string, $encrypt_method, $key, 0, $iv);
        $output = base64_encode($output);
    } 
    else if ($action == 'decrypt') 
    {
        $output = openssl_decrypt(base64_decode($string), $encrypt_method, $key, 0, $iv);
    }
    return $output;
}
```` 
```
