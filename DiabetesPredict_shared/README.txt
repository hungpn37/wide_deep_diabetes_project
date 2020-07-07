1) Dataset chứa trong thư mục trainingSet (17 files csv)
Bảng training_SyncTranscript_cleaned đã clean các outliers về Weight, Height, BMI

2) Chương trình gồm 2 phần:
- DiabetesPredict_ReadData.ipynb
- DiabetesPredict_WideDeep.ipynb

3)DiabetesPredict_ReadData.ipynb:
- Thực hiện đọc dữ liệu từ dataset, trích chọn đặc trưng 
- Cài đặt hiện tại đang lấy 1311 features, có thể điều chỉnh tham số để lấy thêm hoặc bớt đặc trưng
(Kết quả lưu vào file patientdata_1311.csv trong thư mục trainingSet)
- Các features gồm:
	#Phần 1: Các features cơ bản lấy cố định (tổng cộng 53 features) bao gồm:
	-- Age, gender
	-- Nhóm features về BMI: BMImean, BMImin, BMImax, BMIDiff, isOverweight, isObese
	-- Nhóm features về BloodPressure
	-- specials features (các bệnh đặc biệt theo nhóm mã ICD9Code)
	-- ICD9labels features (nhóm theo mã ICD9Code)
	
	#Phần 2: Các features có thể tùy chỉnh để lấy thêm/bớt số lượng, bao gồm:
	-- diagnosis features
	-- medication features 
	-- lab tests features
	(3 nhóm features phần này được mã hóa label encode thành binary vector và sẽ đưa vào 3 embeddings tương ứng)
	
	#Phần 3: crossed features
	-- Chọn (đặt ngưỡng) các top diagnosis features để crossed với top medication features 

4)DiabetesPredict_WideDeep.ipynb:
- Cài đặt wide deep model và thực hiện training, testing với dataset (đọc dữ liệu từ file đặc trưng đã lưu trên)
- Dataset được đọc từ file đặc trưng đã có, tách các phần theo dải index các columns
- Stratified Split thành TRAIN_VAL/TEST set (0.7/0.3) 
- Minmax scale các continuous columns (age, bmi, ...)

#Phần TRAINING:
- Split 5-fold, có thể sử dụng SMOTE cho phần TRAIN (code comment)
- Các model được save trong thư mục model (save 1 file temp + 5 file models)
- File train_log.csv

#Phần TESTING:
- Load các models đã save trong thư mục model tương ứng
- Đo đạc kết quả tập TEST (30% samples) qua từng model gồm:
	-- Accuracy score
	-- AUC score
	-- Sensitivity
	-- Specificity
- Dùng ensemble 5 model bằng các tính probability mean để được 1 ensemble model
- Đo đạc kết quả TEST samples chạy qua ensemble model
- Kết quả test được lưu trong file test_result.csv

5) Mỗi lần chạy xóa các file trong thư mục model để không test lại model cũ