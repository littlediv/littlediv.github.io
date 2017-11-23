# Android 7.0 调用系统照相功能 应用崩溃 #

## android.os.FileUriExposedException: file:////storage/emulated/0/XXX.jpg exposed beyond app through Intent.getData() ##


## Failed to find configured root that contains /storage/emulated/0/Android/data/com.xinhong.analysis/files ##


Log.e(TAG, "selectFileFromLocal: "+ filesDir.getAbsolutePath() ); 

？？？？？

selectFileFromLocal: /data/user/0/com.xinhong.analysis/files

代码： 
File filesDir = getActivity().getFilesDir();
//        File filesDir = Environment.getExternalStorageDirectory();
        if (filesDir == null || !filesDir.exists()) {
            ToastUtils.showToast(getActivity(), "file open failed");
            return;
        }
        Uri fileUri = FileProvider.getUriForFile(getActivity(), "com.xinhong.analysis.fileprovider", filesDir);
        Log.e(TAG, "selectFileFromLocal: "+ filesDir.getAbsolutePath() );


        Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
        intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
        intent.addCategory(Intent.CATEGORY_DEFAULT);
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        intent.setDataAndType(fileUri, "file/*");

        startActivity(intent);

清单文件：

  <provider android:authorities="com.xinhong.analysis.fileprovider"
            android:name="android.support.v4.content.FileProvider"
            android:exported="false"
            android:grantUriPermissions="true"
            >

            <meta-data android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths"
                ></meta-data>
        </provider>

xml/file_paths

<?xml version="1.0" encoding="utf-8"?>
<paths >
    <files_path name="file" path=""></files_path>
</paths>