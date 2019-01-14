# PHP Date From Created 
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
