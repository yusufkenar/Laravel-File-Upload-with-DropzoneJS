Laravel Dropzone Multi File Upload
DropzoneJS: [https://github.com/enyo/dropzone](https://github.com/enyo/dropzone)
Documentation:[https://www.dropzonejs.com](https://www.dropzonejs.com/)
View

    <form method="post" action="{{ route('photos.save') }}" enctype="multipart/form-data"  
          class="dropzone" id="dropzone">  
          @csrf  
          <div class="dz-message needsclick">  
          Drop Files here or click to upload.<br/>  
         <span class="note needsclick">(Accept file types: <strong>JPEG, JPG, PNG</strong>)</span>  
    @section('styles')    
    <link href="{{ fasset('vendor/dropzone/dropzone.css') }}" rel="stylesheet">
         </div></form>  
    @endsection  
    @section('scripts')
         <script src="{{ fasset('vendor/dropzone/dropzone.js') }}"></script>  
    <script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.10.3/jquery-ui.min.js"></script>  
      
    <script type="text/javascript">  
      const Toast = Swal.mixin({  
            toast: true,  
      position: 'bottom-end',  
      showConfirmButton: false,  
      timer: 3000  
      });  
      Dropzone.options.dropzone =  
	  {  
	      maxFilesize: 12,  
	      acceptedFiles: ".jpeg,.jpg,.png", // Accepted File Types (Only Image) 
	      disablePreview: true,  
	      addRemoveLinks: false,  
	      timeout: null,  
	      success: function (file, response) {  
	        // Success Actions 
	      },  
	      error: function (file, response) {  
			//Error Actions
			return false;
	     }
     };
     @endsection

Route

    $router->group(['prefix' => 'Photos', 'as' => 'photos.'], function (Router $router) {  
      $router->post('/Save', ['as' => 'store', 'uses' => 'PhotoController@store']);  
    });

Controller 

    if (isset($photo)) {  
      $normalFilePath = 'upload/location/';  
     if (!File::exists($normalFilePath)) {  
     File::makeDirectory($normalFilePath);  
      }  
      $photoname = uniqid('P_') . ".png";  
      Image::make(file_get_contents($photo))->save($normalFilePath . $photoname);  
      Image::make(file_get_contents($photo))  
     ->resize(null, 80, function ($constraint) {  
      $constraint->aspectRatio();  
      $constraint->upsize();  
      })->save($normalFilePath . '80/' . $photoname);  
      // Save DB  Actions
    }

