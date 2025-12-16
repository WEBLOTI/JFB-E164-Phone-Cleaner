# FB-E164-Phone-Cleaner
PHP solution for JetFormBuilder that implements rigorous cleaning (Custom Transform) of the Phone field. It removes formatting characters (parentheses, hyphens, spaces) to ensure that data is sent in E.164 format (e.g., +15551234567) to webhooks from services such as Twilio and Make, preventing failures in SMS automations.

# 📞 Solución: Limpieza E.164 para Campo Teléfono en JetFormBuilder
Este repositorio documenta una solución para el problema común de envío de datos de números de teléfono con máscara de entrada (ej: `+1 (555) 123-4567`) a servicios que requieren el formato internacional **E.164** (ej: `+15551234567`), como Twilio o pasarelas de SMS, al utilizar Webhooks con **Make (Integromat)**.

La solución se implementa utilizando la funcionalidad **`Custom Transform`** de JetFormBuilder (JFB).

## 1. ⚠️ El Problema
Cuando se utiliza un campo `Tel` en JetFormBuilder con una **Máscara de Entrada**, JFB almacena y envía el valor **con los caracteres de formato** (espacios, guiones, paréntesis) a través del Webhook.

Esto provoca que servicios como Twilio (para el envío de SMS de confirmación) rechacen la solicitud, ya que no se adhiere al formato estricto E.164, causando fallos constantes en la automatización de Make.

## 2. ✅ La Solución: Custom Transform (PHP)
En lugar de depender de las opciones de sanitización estándar de JFB, creamos una función PHP personalizada que limpia rigurosamente el dato para dejar solo el signo `+` y los dígitos.

### Paso A: Agregar la Función PHP (Al Servidor)
Copie y pegue el siguiente código PHP en el archivo `functions.php` de su tema hijo o utilizando un plugin de *snippets* de código:

```php
/**
 * Nombre de la función: jet_fb_sv_phone_e164_clean
 * Propósito: Limpiar el número de teléfono al formato E.164 (Twilio compatible).
 * Elimina todos los caracteres que no sean dígitos (0-9) o el signo + inicial.
 *
 * @param \JFB_Modules\Block_Parsers\Field_Data_Parser $parser
 */
function jet_fb_sv_phone_e164_clean( \JFB_Modules\Block_Parsers\Field_Data_Parser $parser ) {
    $value = $parser->get_value();

    // La expresión regular mantiene el signo '+' SOLO al principio (si existe) y los dígitos.
    // Elimina todos los paréntesis, espacios, guiones, y cualquier otro carácter no deseado.
    $value = preg_replace( '/[^0-9\+]+/', '', $value );
    
    // Opcional: Si el número es demasiado corto (ej: menos de 7 dígitos), lo descartamos.
    if ( empty( $value ) || strlen( $value ) < 7 ) {
        $parser->set_value( '' );
        return;
    }

    $parser->set_value( $value );
}
