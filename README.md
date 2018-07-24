# Документация к апи.

### Авторизация запроса. 
Псевдокод: 
Вход: ассоциативный массив/map/dictionary параметров. пример ["p1" : "value1", "p3": "value3", "p2" : "value2", "xp" : "val4"]
1. добавить выданый Вам токен в массив: ["p1" : "value1", "p3": "value2", "p2" : "value2", "xp" : "val4", "token":"mytoken"]
2. отсортировать массив по ключу в алфавитном порядке [a-z]: ["p1":"value1", "p2":"value2", "p3":"value3", "token":"mytoken", "xp" : "val4"]
3. сериализовать массив в json строку: {"p1":"value1","p2":"value2","p3":"value3","token":"mytoken","xp":"val4"}
4. добавить выданый Вам secret_key к концу json строки: {"p1":"value1","p2":"value2","p3":"value3","token":"mytoken","xp":"val4"}mySecretKey
5. посчитать md5 хэш от этой строки, этот хэш является подписью запроса: fa5cbe3f66ef2de6a76047aecb9b521a
6. добавить подпись к массиву параметров по ключу sign ["p1" : "value1", "p3": "value2", "p2" : "value2", "xp" : "val4", "token":"mytoken", "sign":"fa5cbe3f66ef2de6a76047aecb9b521a"]
7. можно отправлять на сервер, что получили в (6) как urlform-encoded или multipart/form-data

Пример реализации на PHP:
```php
        $secret_key = 'sec';
        $token = "tok";
        
        $params = ["p1" => "v1", "p2" => "v2", "p0" => "v0", 'token' => $token];
        
        ksort($params);
        $params_json = json_encode($params);
        $sign = md5($params_json . $secret_key);
        
        $params['sign'] = $sign;
        
        echo http_build_query($params);
```

### Фиксация подозреваемого в видео архиве /api/archive_event

Отправляете, каждый раз как фиксируете подозреваемого в видеоархиве.

| параметр | тип | описание | 
| ------ | ---- | ------ |
| sespect_id | int | идентификатор подозреваемого |
| archive_name | string | имя файла архива |
| offset | HH:MM:SS | время от начала архива |
| frame | int | номер кадра от начала архива |
| bbox | string | bounding box  подозреваемого в формате: top_left_x,top_left_y,width,height|
| token | string | ваш токен |
| sign | string | подпись запроса |
