![alt text](https://res.cloudinary.com/dgxdamqhe/image/upload/v1545168182/logo_wc_png_irc4l2.png)

# Config Storage in config/storage.yml

```
amazon:
  service: S3
  access_key_id: YOURACCESSKEYID
  secret_access_key: YOURSECRETACCESSKEY
  region: region_your_bucket
  bucket: your_bucket_name

```

# Config Development in

config/environments/development.rb

```
config.active_storage.service = :amazon

```

# If you want to upload more than 1 image at a time, change the following files

app/models/post.rb

Before
```
class Post < ApplicationRecord
  has_one_attached :image
end
```

After
```
class Post < ApplicationRecord
  has_many_attached :images
end
```

app/controllers/posts_controller.rb

Before
```
def post_params
  params.require(:post).permit(:title, :body, :image)
end
```

After
```
def post_params
  params.require(:post).permit(:title, :body, images: [])
end
```

app/views/posts/_form.html.erb

Before
```
<div class="field">
  <%= form.label :image %>
  <%= form.file_field :image %>
</div>
```

After
```
<div class="field">
  <%= form.label :images %>
  <%= form.file_field :images, direct_upload: true, multiple: true %>
</div>
```

Refs
https://www.lucascaton.com.br/2018/04/14/video-aprenda-active-storage-parte-1/
https://www.lucascaton.com.br/2018/04/14/video-aprenda-active-storage-parte-2
