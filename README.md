# PHP/laravel Date and Zip Create and Download and PHP CURL

### Show date in laravel bladed
```php

{{date('l jS F Y ', strtotime($user->created_at))}}
```
```php
  $time_ago = strtotime($notify->created_at);
  $cur_time   = time();
  $time_elapsed   = $cur_time - $time_ago;
  $seconds    = $time_elapsed ;
  $minutes    = round($time_elapsed / 60 );
  $hours      = round($time_elapsed / 3600);
  $days       = round($time_elapsed / 86400 );
  $weeks      = round($time_elapsed / 604800);
  $months     = round($time_elapsed / 2600640 );
  $years      = round($time_elapsed / 31207680 );
  // Seconds
  if($seconds <= 60)
  {
      echo "just now";
  }
  //Minutes
  else if($minutes <=60)
  {
      if($minutes==1)
      {
          echo "one minute ago";
      }
      else
      {
          echo "$minutes minutes ago";
      }
  }
  //Hours
  else if($hours <=24)
  {
      if($hours==1)
      {
          echo "an hour ago";
      }
      else
      {
          echo "$hours hrs ago";
      }
  }
  //Days
  else if($days <= 7)
  {
      if($days==1)
      {
          echo "yesterday";
      }
      else
      {
          echo "$days days ago";
      }
  }
  //Weeks
  else if($weeks <= 4.3)
  {
      if($weeks==1)
      {
          echo "a week ago";
      }
      else
      {
          echo "$weeks weeks ago";
      }
  }
  //Months
  else if($months <=12)
  {
      if($months==1)
      {
          echo "a month ago";
      }
      else
      {
          echo "$months months ago";
      }
  }
  //Years
  else{
      if($years==1)
      {
          echo "one year ago";
      }
      else
      {
          echo "$years years ago";
      }
  }
```

## Last day of the month
```php
public function get_date()
    {
    	//Get Last Day Of The Current Month:
    	$lastDay = date('t',strtotime('today'));
    	print_r($lastDay);

    	//Get Last Day Of The Current Month 2:
    	$lastDateOfThisMonth =strtotime('last day of this month') ;
    	$lastDay = date('d/m/Y', $lastDateOfThisMonth);
    	print_r($lastDay);

    	//Get Last Day Of The Next Month:
    	$lastDay = date('t',strtotime('next month'));
    	print_r($lastDay);

    	//Get Last Day Of The Next Month 2:
    	$lastDateOfNextMonth =strtotime('last day of next month') ;
    	$lastDay = date('d/m/Y', $lastDateOfNextMonth);
    	print_r($lastDay);

    	//Get Last Day Of The Previous Month:	
    	$lastDay = date('t',strtotime('last month'));
    	print_r($lastDay);


    	//Get Last Day Of The Previous Month 2:
    	$lastDateOfLastMonth =strtotime('last day of last month') ;
    	$lastDay = date('d/m/Y', $lastDateOfLastMonth);
    	print_r($lastDay);

    	//Get Last Day Of The Specific Month:
    	$lastDay = date('t',strtotime('1/1/2018'));
    	print_r($lastDay);
    }

```
## PHP CURL 
use this code in helper in your project
```php
function callMailchimpAPI($method, $url, $data=null)
{
    $curl = curl_init();
    /*$info = curl_getinfo($curl);
    echo "<pre>";
    print_r( $info );*/
    switch ($method)
    {
        case "POST":
            curl_setopt($curl, CURLOPT_POST, 1);
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
         break;
        case "PATCH":
            curl_setopt($curl, CURLOPT_CUSTOMREQUEST, "PATCH");
        if ($data)
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);                              
        break;
        case "DELETE":
            curl_setopt($curl, CURLOPT_CUSTOMREQUEST, "DELETE"); 
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
        default:
        if($data)
            $url = sprintf("%s?%s", $url, http_build_query($data));
   }
    
    $headers = array(  
        "--user: 93353d176c81e3391109a76455f50045-us7",
        "Authorization: Basic b3dhaXNfdGFhcnVmZjo5MzM1M2QxNzZjODFlMzM5MTEwOWE3NjQ1NWY1MDA0NS11czc=",
        "Content-Type: application/json",
        "Postman-Token: 163f7134-68ca-45bb-9420-ebf2bef7f447",
        "cache-control: no-cache"
     );
    curl_setopt_array($curl, 
    [
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_SSL_VERIFYPEER => false,
        CURLOPT_SSL_VERIFYHOST => false,
        CURLOPT_HTTPHEADER => $headers, 
        CURLOPT_HEADER => false,
        CURLOPT_URL => $url.'?apikey=93353d176c81e3391109a76455f50045-us7'
    ]);

    $response = curl_exec($curl);
    curl_close($curl);
    return $response;
}

function createMailChimpStore($id)
{
    $curl = curl_init();
    $store_name = "store_";
    curl_setopt_array($curl, array(
    CURLOPT_URL => "https://us7.api.mailchimp.com/3.0//ecommerce/stores/",
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => "",
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 30,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => "POST",
    CURLOPT_POSTFIELDS => '{
      "id" : "'.$store_name.$id.'",
      "list_id" : "317fbacff8",
      "name" : "'.$store_name.$id.'",
      "domain" : "http://boksha.eshmar.com/storew/inspiration",
      "email_address" : "inspiratiossn@boksha.com",
      "currency_code" : "AED"
    }',
     CURLOPT_HTTPHEADER => array(
        "--user: 93353d176c81e3391109a76455f50045-us7",
        "Authorization: Basic b3dhaXNfdGFhcnVmZjo5MzM1M2QxNzZjODFlMzM5MTEwOWE3NjQ1NWY1MDA0NS11czc=",
        "Content-Type: application/json",
        "Postman-Token: 8621048d-e026-4135-b456-400b3f3ec523",
        "cache-control: no-cache"
      ),
    ));

    $response = curl_exec($curl);
    $err = curl_error($curl);

    curl_close($curl);
}

/* Store Product */
function curlMailchimpStore($data_array, $product)
{
    $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores";
    $data = callMailchimpAPI('GET', $url, false);
    $result = json_decode($data);

    $store_ids = array();
    foreach ($result->stores as $store) 
    {
         $store_ids[] =  $store->id;            
    }
    /* Distinct Srore */
    if (in_array("store_".$product->title, $store_ids))
    {
        $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_$product->title/products";
        $data = callMailchimpAPI('POST', $url, $data_array);
    }
    else
    {
        createMailChimpStore($product->title);
        $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_$product->title/products";
        $data = callMailchimpAPI('POST', $url, $data_array);
    }
    /* Common store */
    if (in_array("store_all_products", $store_ids))
    {
        $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_all_products/products";
        $data = callMailchimpAPI('POST', $url, $data_array);
    }
    else
    {
        $id = 'all_products';
        createMailChimpStore($id);
        $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_all_products/products";
        $data = callMailchimpAPI('POST', $url, $data_array);
    }
}

/* Update Product */
function curlMailchimpUpdate($data_array, $product)
{

    $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_$product->title/products/$product->id";
    $data = callMailchimpAPI('PATCH', $url, $data_array);

    $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_all_products/products/$product->id";
    $data = callMailchimpAPI('PATCH', $url, $data_array);
}
    /*Delete Product */
function curlMailchimpDelete($id)
{
    $delete_id = curl_product_query($id);
    $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_$delete_id->title/products/$id";
    $data = callMailchimpAPI('DELETE', $url, false);

    $url = "https://us7.api.mailchimp.com/3.0/ecommerce/stores/store_all_products/products/$id";
    $data = callMailchimpAPI('DELETE', $url, false);
}

function curl_data_array($product)
{
    $product_url = "http://boksha.com/product/$product->slug";
    return  $data_array = '
    {
            "id": "'.$product->id.'", 
            "title": "'.$product->name.'", 
            "handle": "'.$product->slug.'", 
            "url": "'.$product_url.'", 
            "description": "'.$product->description.'", 
            "type": "'.$product->slug.'",
            "vendor": "'.$product->title.'", 
            "image_url": "'.$product->image_path.'", 
            "variants": [
                { 
                    "id": "'.$product->id.'",
                    "title": "'.$product->name.'",
                    "url": "'.$product_url.'",
                    "sku": "",
                    "price": "'.$product->price.'",
                    "inventory_quantity": 0,
                    "image_url": "'.$product->image_path.'",
                    "backorders": "0",
                    "visibility": "visible",
                    "created_at": "'.$product->created_at.'",
                    "updated_at": "'.$product->updated_at.'"
                }
            ]
     }';
}

function curl_product_query($id)
{
    return $product = DB::table('products')
        ->join('stores', 'stores.id', '=', 'products.store_id')
        ->leftJoin('product_images', 'products.id', 'product_images.product_id')
        ->select('products.*', 'stores.title','product_images.image_path')
        ->whereNull('products.deleted_at')
        ->groupBy("products.id")
        ->where('products.id',  $id)
        ->first();
}

```
### use this in method
```php
public function store(ProductsRequest $request) {
    $last_inserted_id = $product->id;
    $product = curl_product_query($last_inserted_id);
    $data_array = curl_data_array($product);
    curlMailchimpStore($data_array, $product);
}
```
### Zoom Image
click here to read zoom image 

(http://mark-rolich.github.io/Magnifier.js/)[http://mark-rolich.github.io/Magnifier.js/]

## Multiple images download and delete in Laravel/Codeigniter
```js
$(document).ready(function () 
{
    $.ajaxSetup({
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
        }
    });

    $('#download-master').on('click', function(e) 
    {
        if($(this).is(':checked',true))  
        {
            $(".multiple_download").prop('checked', true);  
        } else {  
            $(".multiple_download").prop('checked',false);  
        }  
    });

    $('.download-all').on('click', function(e) {

        var allVals = [];  
        $(".multiple_download:checked").each(function() {  
            allVals.push($(this).attr('data-id'));
        });  

        if(allVals.length <=0)  
        {  
            alert("Please select row to download.");  
        }  
        else 
        {  
            var join_selected_values = allVals.join(",");
            $.ajax({
                url: $(this).data('url'),
                type: 'POST',
                data:'_token = <?php echo csrf_token() ?>',
                data: 'ids='+join_selected_values,
                success: function (data) 
                {
                    //data.fileurl,
                    //window.location = response;
                    alert('Seccussfully Downloaded');
                },
                error: function (data) {
                    alert(data.responseText);
                }
            }); 
        }  
    });

    $('.download-all').on('click', function(e){
        e.preventDefault();
        var allVals = [];  
        $(".multiple_download:checked").each(function() {  
            allVals.push($(this).attr('data-id'));
        });  

        if(allVals.length <=0)  
        {  
            console.log("Please select row to download.");  
        }
        else
        {
            $('.zip-file').multiDownload();
        }
        
    });
});

```
### MultiDownload js
```js
(function ($) {

    var methods = {
        _download: function (options) {
            var triggerDelay = (options && options.delay) || 100;
            var cleaningDelay = (options && options.cleaningDelay) || 1000;

            this.each(function (index, item) {
                methods._createIFrame(item, index * triggerDelay, cleaningDelay);
            });
            return this;
        },

        _createIFrame: function (item, triggerDelay, cleaningDelay) {
            setTimeout(function () {
                var frame = $('<iframe style="display: none;" class="multi-download-frame"></iframe>');
                frame.attr('src', $(item).attr('href') || $(item).attr('src'));
                $(item).after(frame);
                setTimeout(function () { frame.remove(); }, cleaningDelay);
            }, triggerDelay);
        }
    };

    $.fn.multiDownload = function(options) {
        return methods._download.apply(this, arguments);
    };

})(jQuery);
```

### Code
```php
public function downloads_all(Request $request) 
     {
        $public_dir = '';
        $$zipFileName = '';
        $zip = '';
        $product_images =  DB::table('product_images')
                    ->where('deleted_at', null)
                    ->whereIn('product_id',  explode(',', $request->ids))
                    ->get();
        if ($product_images) 
        {
            foreach ($product_images as $value) 
            {
                $zipFileName = 'product_' . $value->product_id . '_images.zip';
                $zip = new ZipArchive;
                $public_dir = public_path('uploads/download-images');
                if (!is_dir($public_dir)) 
                {
                    mkdir($public_dir, 0777, true);
                }
                if ($zip->open($public_dir . '/' . $zipFileName, ZipArchive::CREATE) === TRUE) 
                {
                    foreach ($product_images as $image) 
                    {
                        $directory_path = base_path() . '/public/uploads/products/' . $image->product_id . '/images/';
                        $image_path = $image->image_path;
                        $file_name = basename($image_path);
                        $zip->addFile($directory_path . $file_name, $file_name);
                    }
                    $zip->close();
                }
            }
            $files = glob(public_path('uploads/download-images'));
            \Zipper::make(public_path('uploads/product_images.zip'))->add($files)->close();


            $fileurl =  public_path('uploads/product_images.zip');
    
            
            if (file_exists($fileurl)) 
            {
                return 'success';
                //return response()->download($fileurl);
            }
            else
            {
                return back();
            }
            
           /* $headers = [
                'Access-Control-Allow-Origin' => '*',
                'Access-Control-Allow-Methods' => 'POST, GET, OPTIONS, PUT, PATCH, DELETE',
                'Access-Control-Allow-Headers' => 'Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Authorization , Access-Control-Request-Headers',
                'Content-Type' => 'application/zip'
                ];
            return response()->download(public_path('uploads/'), 'product_images.zip', $headers);*/
        }
        else
        {
            return back();
        }
    }
```
### views
```html
<div class="col-md-2">
  <input type="button" id="download-button" value="Download" class="btn sbold green float-right download-all" data-url="{{url('admin/product/download-all')}}" style="margin-left: 12px !important;">
</div>
<a class="zip-file" href="{{ url('public/uploads/product_images.zip') }}"></a>

<td>
  <label class="mt-checkbox mt-checkbox-single mt-checkbox-outline">
    <input type="checkbox" class="checkboxes bulk-checkbox multiple_download" value=" {{$product->id}}" name="product_ids[]" data-id="    {{$product->id}}"/>
<span></span>
  </label>
</td>

````
### Read file from folder in laravl
```php
$files = File::files(public_path('uploads/download-images'));
if ($files !== false) 
{
    $filecount = count($files);
   foreach ($filecount as $file) {
        return response()->download($filetopath, $zipFileName, $headers);
   }
}
```
### Multiple rows update and insert in laravel
 ```php
 function update_financial_report(Request $request)
    {
        $order_item_id = $request->order_item_id;
        $financial_notes = $request->financial_notes;
        $financial_comments = $request->financial_comments;
        $transfer_amount = $request->transfer_amount;
        $transfer_date = $request->transfer_date;

        foreach($order_item_id as $k => $id){
          $values = array(
                'financial_notes'=> $financial_notes[$k],
                'financial_comments'=> $financial_comments[$k],
                'transfer_amount'=> $transfer_amount[$k],
                'transfer_date'=> $transfer_date[$k]
            );
          OrderToStore::where('id',$id)->update($values);

        }
        return redirect()->back();
    }
 ````
### Html 
```html
<input type="hidden" value="<?php echo $order->order_item_id; ?>" name="order_item_id[]">
 <input type="text" name="transfer_amount[]" value="<?php echo $net_transfer_store; ?>">
```

# Multiple Image upload
```php
public function vehicle_verification(Request $request)
    {

        $files=[];
        $destinationPath = public_path('/uploads/vehicle_verification');
        if (!is_dir($destinationPath)) 
        {
            mkdir($destinationPath, 0777, true);
        }
        foreach ($request->file('dv_vehicle_img') as $image) 
        {
            if (!empty($image)) 
            {
                $fileName = $image->getClientOriginalName();
                $image->move($destinationPath,$fileName);
                //dd($request->all());
                $files[] = '/uploads/vehicle_verification/'.$fileName;
            }

        }

        
        DB::table('tms_vehicle_detail')
            ->where('dv_vehicle_id', $request->dv_vehicle_id)
            ->update([
                'dv_vehicle_img' =>  implode(',',$files),
                'remarks' =>  $request->remarks,
                'is_verify' =>  $request->is_verify,
            ]);
        return redirect()->back();
    }

```
