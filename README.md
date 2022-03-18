# OpencartEklenti
Opencart 3.x eklenti geliştiriken iki kısım mevcuttur. bir kısım admin diğer kısım ise catalog kısmı , dosyalama sistemi resimdeki gibidir.
------
<img width="298" alt="Screen Shot 2022-03-18 at 10 06 59 AM" src="https://user-images.githubusercontent.com/101548542/158954179-3a8e4722-e243-4127-9e2a-7625ced61f5c.png">
------

Admin > controller > extension > payment > buraya dosya ismini yazıyoruz.
payment klasörü ise payment kısmına ekleyeceğimiz için ekliyoruz dosyayı
```
public function index() {
$this->load->language('extension/payment/payment');
$this->load->model('extension/payment/payment');
}
```
Yukarı da ise dosyaları burada tanımlıyoruz.Css veya js tanımlamak için ise 
```
public function index() {
  $this->document->addStyle('view/stylesheet/cssName.css');
}
```
Şimdi ise index function içine datalarımızı nereden çekeceğimizi gösteriyoruz.

```
public function index() {
 $data['name']  = $this->language->get('name');
}
```
Language>tr-tr>extension>payment>payment.php
dil dosyasına gelerek buraya ekleme yapıyoruz.
```
$_['name'] = 'name';

```
view klasörüne ise css , image , js theme ile ilgili özelleştirmeleri buraya ekleyebilirsiniz.Opencart 3.x ile dosya isimlerin sonlarına .twig uzantısı geldi buraya html kodlarını ekleyip içlerine laravel de olduğu gibi {{ name }} şeklinde ekliyoruz.


```
<table>
  <tr>
    <th>isim</th>
  </tr>
  <tr>
    <th>{{ name }}</th>
  </tr>
</table>

```

Model dosyamız içinde ise veri tabanı işlemlerini yapıyoruz.Burada fonksiyonlarınza install veya uninstall verirseniz yükleme veya silme durumlarında kolaylık sağlar.


```
public function install() {
        $this->db->query("
			CREATE TABLE IF NOT EXISTS `" . DB_PREFIX . "name` (
			  `id` INT(11) NOT NULL AUTO_INCREMENT,
			  `name` VARCHAR(255) NOT NULL,
			  
			)  DEFAULT COLLATE=utf8_general_ci;");

        
    }
    
      public function uninstall() {
        $this->db->query("DROP TABLE IF EXISTS `" . DB_PREFIX . "name`;");
       
    }
   

```
Yükleme esnasında veri tabanı içinde tablo mevcut değil ise kurulum yapacaktır.Eklenti silindiğinde ise otomatik silinecektir.

Burada ise veri tabanından bilgiyi çektik.

```
 $name = array();
		$query = $this->db->query("SELECT * FROM " . DB_PREFIX . "name");
		foreach ($query->rows as $result) {
			$name[] = $result['name'];
		}
		return $name;
```
