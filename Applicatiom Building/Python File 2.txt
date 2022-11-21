def upload():
    if request.method == "POST":
        f = request.files["image"]
        filepath = secure_filename(f.filename)
        f.save(os.path.join(app.config['UPLOAD_FOLDER'], filepath))
        upload_img = os.path.join(UPLOAD_FOLDER, filepath)
        img = Image.open(upload_img).convert("L")  
        img = img.resize((28, 28))  
        im2arr = np.array(img) 
        im2arr = im2arr.reshape(1, 28, 28, 1)  
        pred = model.predict(im2arr)
        num = np.argmax(pred, axis=1)  
        return render_template('/predict.html', num=str(num[0]))