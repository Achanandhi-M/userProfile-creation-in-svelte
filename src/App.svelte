<script>
    import { onMount } from 'svelte';
    import Cropper from 'cropperjs';
    import 'cropperjs/dist/cropper.css';
  
    let image;
    let cropper;
  
    const uploadImage = () => {
      const input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.addEventListener('change', (event) => {
        const file = event.target.files[0];
        const reader = new FileReader();
        reader.addEventListener('load', () => {
          image.src = reader.result;
          cropper = new Cropper(image, {
            aspectRatio: 1,
            viewMode: 1,
          });
        });
        reader.readAsDataURL(file);
      });
      input.click();
    };
    
    const deleteImage = () => {
      image.src = '';
      cropper.destroy();
    };
    
    const handleMouseEnter = () => {
      image.style.filter = 'brightness(70%)';
    };
    
    const handleMouseLeave = () => {
      image.style.filter = '';
    };
    
    const rotateLeft = () => {
      cropper.rotate(-90);
    };
    
    const rotateRight = () => {
      cropper.rotate(90);
    };
    const saveImage = () => {
      const canvas = cropper.getCroppedCanvas();
      const croppedImage = canvas.toDataURL();
      image.src = croppedImage;
      cropper.destroy();
    };
  
    onMount(() => {
      image = document.getElementById('profile-image');
    });
  </script>
  <style>
  .card-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  width: 250px;
  height: 250px;
  background-color: #F5F5F5;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  position: relative;
}

#profile-image {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
  transition: filter 0.2s ease-in-out;
}

.edit-options {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

.edit-options button {
  background-color: #FFFFFF;
  border: none;
  color: #333333;
  padding: 0.5rem;
  display: flex;
  border-radius: 5px;
  cursor: pointer;
}

.card:hover #profile-image {
  filter: brightness(70%);
}

.card:hover + .edit-options {
  display: flex;
}

  </style>
  <div class="card-container">
    <div class="card">
      <img id="profile-image" src="" alt="Profile" on:mouseenter={handleMouseEnter} on:mouseleave={handleMouseLeave}>
    </div>
    <div class="edit-options">
      <button on:click={uploadImage}>Upload Image</button>
      <button on:click={rotateLeft}>Rotate Left</button>
      <button on:click={rotateRight}>Rotate Right</button>
      <button on:click={saveImage}>Save</button>
      <button on:click={deleteImage}>Cancel</button>
    </div>
</div>