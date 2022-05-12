# DBpackage com.example.myapplication23;

import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;

import android.database.Cursor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    EditText name,dob,contact;
    Button insert,update,delete,view;
    DBHelper db;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        name=findViewById(R.id.Name);
        dob=findViewById(R.id.dob);
        contact=findViewById(R.id.contact);
        insert=findViewById(R.id.insert);
        delete=findViewById(R.id.delete);
        update=findViewById(R.id.update);
        view=findViewById(R.id.view);
        db= new DBHelper(this);

        insert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String nameTXT=name.getText().toString();
                String contactTXT=contact.getText().toString();
                String dobTXT=dob.getText().toString();

                Boolean checkinsertdata=db.insertuserdata(nameTXT,contactTXT,dobTXT);

                if(checkinsertdata==true)
                    Toast.makeText(MainActivity.this, "New Entry Inserted",Toast.LENGTH_SHORT).show();
                else
                    Toast.makeText(MainActivity.this, "Not Inserted",Toast.LENGTH_SHORT).show();

            }
        });
        update.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String nameTXT=name.getText().toString();
                String contactTXT=contact.getText().toString();
                String dobTXT=dob.getText().toString();

                Boolean checkupdatedata=db.updateuserdata(nameTXT,contactTXT,dobTXT);
                if(checkupdatedata==true)
                    Toast.makeText(MainActivity.this, "New Entry Updated",Toast.LENGTH_SHORT).show();
                else
                    Toast.makeText(MainActivity.this, "Not Updated",Toast.LENGTH_SHORT).show();

            }
        });
        delete.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String nameTXT=name.getText().toString();


                Boolean checkdeletedata=db.deletedata(nameTXT);
                if(checkdeletedata==true)
                    Toast.makeText(MainActivity.this, " Entry Deleted",Toast.LENGTH_SHORT).show();
                else
                    Toast.makeText(MainActivity.this, "Not Deleted",Toast.LENGTH_SHORT).show();

            }
        });
        view.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Cursor res=db.getdata();
                if(res.getCount()==0);{
                    Toast.makeText(MainActivity.this,"No entry exists", Toast.LENGTH_SHORT).show();

                }
                StringBuffer buffer = new StringBuffer();
                while(res.moveToNext()){
                    buffer.append("Name:"+res.getString(0)+"\n");
                    buffer.append("Contact:"+res.getString(1)+"\n");
                    buffer.append("Date of Birth:"+res.getString(2)+"\n");
                }
                AlertDialog.Builder builder=new AlertDialog.Builder(MainActivity.this);
                builder.setCancelable(true);
                builder.setTitle("User Entries");
                builder.setMessage(buffer.toString());
                builder.show();
            }
        });
    }
}
