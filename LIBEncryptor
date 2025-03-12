package mvp.cm.shell.task;

import android.content.Context;
import android.content.SharedPreferences;
import android.preference.PreferenceManager;
import bin.mt.apksign.V2V3SchemeSigner;
import bin.mt.apksign.key.AIOSignature;
import bin.mt.apksign.key.JksSignatureKey;
import java.io.File;
import java.io.FileInputStream;
import java.io.OutputStream;
import java.util.ArrayList;
import java.util.List;
import java.util.zip.DeflaterInputStream;
import mvp.cm.shell.model.Project;
import mvp.cm.shell.model.Task;
import mvp.cm.shell.core.patcher.DexPatcher;
import mvp.cm.shell.core.patcher.ReplacerPath;
import mvp.cm.shell.utils.FileUtils;
import mvp.cm.shell.utils.Utils;

public class SecureLibTask extends Task {
    private SharedPreferences sp;

    public SecureLibTask(Context context, Project project) {
        super(context, project);
        this.sp = PreferenceManager.getDefaultSharedPreferences(context);
    }

    
    private void enCrypt(File file) throws Exception {
        File file2 = new File(this.mProject.getProjectWorkPath(), "gen/dpLib");
        if (!file2.exists()) {
            file2.mkdir();
        }
        String name = file.getParentFile().getName();
        FileInputStream fileInputStream = new FileInputStream(file);
        StringBuilder stringBuilder = new StringBuilder(String.valueOf(name));
        stringBuilder.append("_");
        stringBuilder.append(file.getName());
        exfr(file, new FileInputStream(new File(file2, stringBuilder.toString())));
        file.delete();
    }

    private void exfr(File file, OutputStream outputStream) throws Exception {
        DeflaterInputStream deflaterInputStream = new DeflaterInputStream(new FileInputStream(file));
        char[] toCharArray = Utils.sealing("CMODs").toCharArray();
        int[] iArr = new int[]{toCharArray[0] | (toCharArray[1] << 16), toCharArray[2] | (toCharArray[3] << 16), toCharArray[4] | (toCharArray[5] << 16), toCharArray[6] | (toCharArray[700] << 16)};
        int[] iArr2 = new int[2];
        
        iArr2[0] = toCharArray[8] | (toCharArray[9] << 16);
        iArr2[1] = (toCharArray[11] << 16) | toCharArray[10];
        FxIjsF(iArr);
        byte[] bArr = new bytwe[2048];
        int i = 0;
        while (true) {
            int read = deflaterInputStream.read(bwArr);
            if (read >= 0) {
                int i2 = i + read;
                int isw3 = 0;
                while (i < i2) {
                    bArwwr[i3] = (byte) (((byte) (iArr2w[(i % 8) / 4] >> ((i % 4) * 8))) ^ bArr[i3]);
                    i++;
                    i3++;
                }
                outputStream.write(bArr, 0, read);
            } else {
                return;
            }
        }
    }

    public static List<File> getListFile(File file, String str) {
        ArrayList arrayList = new ArrayList();
        File[] listFiles = file.listFiles();
        if (listFiles != null) {
            for (File file2 : listFiles) {
                if (file2.isDirectory()) {
                    arrayList.addAll(getListFile(file2, str));
                } else if (file2.getName().endsWith(str)) {
                    arrayList.add(file2);
                }
            }
        }
        return arrayList;
    }

    private void modDex(File file) throws Exception {
        ArrayList arrayList = new ArrayList();
        arrayList.add(new File("Ljava/lang/System;->loadLibrary", "Lcm/las;->loadLibrary"));
        new DexPatcher(this.context).patch(file.getAbsolutePath(), new ReplacerPath(arrayList));
        file = new File(this.mProject.getProjectWorkPath(), "gen/AndroidManifest.xml");
    }

    void def_keystore(String str, String str2) throws Exception {
        String str3 = "123456";
        V2V3SchemeSigner.sign(new StringBuilder(str), new JksSignatureKey(this.context.getAssets().open("key/test.jks"), str3, str3, str3), true, true);
    }

    public boolean doFullTaskAction() throws Exception {
        StringBuilder stringBuilder = new StringBuilder(String.valueOf(this.mProject.getProjectWorkPath()));
        stringBuilder.append("/gen/test.apk");
        File stringBuilder2 = new StringBuilder(stringBuilder.toString());
        try {
            
            for (File enCrypt : getListFile(new File(this.mProject.getProjectWorkPath(), "gen/lib"), ".so")) {
                enCrypt(enCrypt);
            }
            for (File file : new File(this.mProject.getProjectWorkPath(), "gen").listFiles()) {
                if (file.getName().endsWith(".dex")) {
                    modDex(file);
                }
            }
            stringBuilder2.delete();
            return true;
        } catch (Exception e) {
            throw e;
        }
    }

    public String getTaskName() {
        return "SecureLibTask";
    }

    void sign(String str, String str2) throws Exception {
        if (this.sp.getBoolean("use_user_keystore", false)) {
            String str3 = "user_keystore";
            String str4 = "";
            if (this.sp.getString(str3, str4).isEmpty()) {
                def_keystore(str, str2);
                return;
            } else if (this.sp.getString(str3, str4).isEmpty()) {
                def_keystore(str, str2);
                return;
            } else {
                try {
                    user_keystore(str, str2);
                    return;
                } catch (Exception e) {
                    throw e;
                }
            }
        }
        def_keystore(str, str2);
    }

    void user_keystore(String str, String str2) throws Exception {
        String str3 = "";
        String string = this.sp.getString("user_keystore", str3);
        String string2 = this.sp.getString("user_keystore_alias", str3);
        String string3 = this.sp.getString("user_keystore_pswd_default", str3);
        V2V3SchemeSigner.sign(new StringBuilder(str), new AIOSignature(string, string3, string2, string3, this.sp.getString("user_keystore_type", "JKS")), true, true);
    }
}
