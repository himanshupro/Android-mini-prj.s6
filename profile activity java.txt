package pro.himanshu.cusatreader;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;
import com.google.firebase.database.DataSnapshot;
import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;
import com.google.firebase.database.ValueEventListener;

public class ProfileActivity extends AppCompatActivity implements View.OnClickListener {

    //firebase auth object
    private FirebaseAuth firebaseAuth;

    //view objects
    public String s="";
    private DatabaseReference ref;
    private TextView textViewUserEmail;
    private Button buttonLogout;
    private Button bt1,bt2,bt3,bt4,bt5,bt6,bt7,bt8;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_profile);

        //initializing firebase authentication object
        firebaseAuth = FirebaseAuth.getInstance();

        //if the user is not logged in
        //that means current user will return null
        if(firebaseAuth.getCurrentUser() == null){
            //closing this activity
            finish();
            //starting login activity
            startActivity(new Intent(this, LoginActivity.class));
        }

        //getting current user
        FirebaseUser user = firebaseAuth.getCurrentUser();

        //initializing views
        textViewUserEmail = (TextView) findViewById(R.id.textViewUserEmail);
        buttonLogout = (Button) findViewById(R.id.buttonLogout);

        //displaying logged in user name
        textViewUserEmail.setText("Welcome" +user.getEmail());


        ref = FirebaseDatabase.getInstance().getReference();
        /*DatabaseReference mostafa = ref.child("User").child("-L8VsvP1BSzbQ8lxo-CM").child("college");

        mostafa.addListenerForSingleValueEvent(new ValueEventListener() {
            @Override
            public void onDataChange(DataSnapshot dataSnapshot) {
                String email = dataSnapshot.getValue(String.class);
                textViewUserEmail.setText("Wel" +email);

                //do what you want with the email
            }

            @Override
            public void onCancelled(DatabaseError databaseError) {

            }
        });*/


        DatabaseReference users = ref.child("User");
        users.orderByChild("email").equalTo(user.getEmail()).addListenerForSingleValueEvent(new ValueEventListener() {
            @Override
            public void onDataChange(DataSnapshot dataSnapshot) {

                String st= dataSnapshot.getValue().toString();
                dataSnapshot.getKey();
                char[] ch=st.toCharArray();
                String s1="";
                int i,c=0,fl=0;
                for(i=1;i<ch.length;i++)
                {   if(ch[i]=='=')
                    c++;
                else if (c == 5 && ch[i] !='{' && ch[i]!='}' )
                    s=s+ch[i];
                else if (c == 2 && fl!=1) {
                    if(ch[i]==',')
                        fl=1;
                    else
                    s1 = s1 + ch[i];
                }
                else if(c>5)
                    break;
                }
                textViewUserEmail.setText("WELCOME " +s1);
                Log.d("User",dataSnapshot.getRef().toString());
                Log.d("User",dataSnapshot.getValue().toString());
            }

            @Override
            public void onCancelled(DatabaseError databaseError) {

                Log.d("User",databaseError.getMessage() );
            }
        });



        //adding listener to button
        buttonLogout.setOnClickListener(this);
        bt1=(Button) findViewById(R.id.s1);
        bt2=(Button) findViewById(R.id.s2);
        bt3=(Button) findViewById(R.id.s3);
        bt4=(Button) findViewById(R.id.s4);
        bt5=(Button) findViewById(R.id.s5);
        bt6=(Button) findViewById(R.id.s6);
        bt7=(Button) findViewById(R.id.s7);
        bt8=(Button) findViewById(R.id.s8);


        bt1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(),Sem1R.class));


            }
        });
        bt2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(),Sem2R.class));


            }
        });
        bt3.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                switch (s)
                {
                    case "IT":
                        startActivity(new Intent(getApplicationContext(),S3itR.class));
                        break;
                    case "CSE":
                       // startActivity(new Intent(getApplicationContext(),S3cseR.class));
                        break;
                    case "EC":
                       // startActivity(new Intent(getApplicationContext(),S3ecR.class));
                        break;




                }



            }
        });
        bt4.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(),S4itR.class));


            }
        });
        bt5.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(),S5itR.class));


            }
        });
        bt6.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(),S6itR.class));


            }
        });
        bt7.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(),S7itR.class));


            }
        });
        bt8.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(getApplicationContext(), S8itR.class));


            }
        });
    }

    @Override
    public void onClick(View view) {
        //if logout is pressed
        if(view == buttonLogout){
            //logging out the user
            firebaseAuth.signOut();
            //closing activity
            finish();
            //starting login activity
            startActivity(new Intent(this, LoginActivity.class));
        }
    }
  /*  public void S1data()
    {
        Toast.makeText(getApplication(),"Saved Successfully",Toast.LENGTH_LONG).show();
        startActivity(new Intent(this, Fupload.class));

    }*/
}