# Part 8
## Putting things in a list

In Android we use `Adapters` to add elements to a list. There's different types of Adapters. Common ones are `ArrayAdapter`, `ListAdapter` and `CursorAdapter`. The `ArrayAdapter` and `ListAdapter` both will display the contents of Arrays and Lists respectively.

Meanwhile, the `CursorAdapter` can be used with a ListView to display the contents of a database in our ListView. This is exactly what we'll be using in this project. We can first start by copy and pasting the adapter code. For this tutorial we won't be focusing much on the backend of this project.

First right click on your package and create a new Java class. 
We'll name this `NoteTakingDatabase`.
```
import android.content.ContentValues;
import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

/**
 * Created by tristan on 1/30/18
 */

public class NoteTakingDatabase extends SQLiteOpenHelper {

    private String DATABASE_NAME = "notes";

    NoteTakingDatabase(Context context) {
        super(context, "NOTES_DATABASE", null, 3);
    }

    // This is where we need to write create table statements.
    // This is called when database is created.
    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE notes(_id INTEGER PRIMARY KEY, noteImage TEXT, noteText TEXT, noteDescription TEXT, noteCategory TEXT)");
    }

    void storeNote(SQLiteDatabase db, String path, String text, String description, String category) {
        ContentValues values = new ContentValues();
        values.put("noteImage", path);
        values.put("noteText", text);
        values.put("noteDescription", description);
        values.put("noteCategory", category);

        db.insert(DATABASE_NAME, null, values);
    }

    void updateNote(SQLiteDatabase db, Integer id, String path, String text, String description, String category) {
        ContentValues values = new ContentValues();
        values.put("noteImage", path);
        values.put("noteText", text);
        values.put("noteDescription", description);
        values.put("noteCategory", category);

        db.update(DATABASE_NAME, values, "_id=?", new String[]{String.valueOf(id)});
    }

    void deleteNote(SQLiteDatabase db, Integer id) {
        this.getWritableDatabase().delete(DATABASE_NAME, "_id=?", new String[]{String.valueOf(id)});
    }

    // This method will simply replace the database if it's out of date.
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion,
                          int newVersion) {
        if (oldVersion != newVersion) {
            db.execSQL("DROP TABLE IF EXISTS notes");
            onCreate(db);
        }
    }
}
```

This file is our database file. It manages the creation, removal, updating, and deleting (CRUD) of our database.

Next we need to create our Adapter. This we'll look at more closely.

## The Adapter
Now let's create our Adapter. This adapter will extend `CursorAdapter` so we can manage the access to our database.

Now let's create the following class called `NoteAdapter`.

```
import android.content.Context;
import android.database.Cursor;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.CursorAdapter;
import android.widget.ImageView;
import android.widget.TextView;

public class NoteAdapter extends CursorAdapter {
    public NoteAdapter(Context context, Cursor c, int flags) {
        super(context, c, flags);
    }

    //Tell the adapter we want to use the note_list_item layout
    @Override
    public View newView(Context context, Cursor cursor, ViewGroup viewGroup) {
        return LayoutInflater.from(context).inflate(R.layout.note_list_item, viewGroup, false);
    }

    @Override
    public void bindView(View view, Context context, Cursor cursor) {
        // Find fields to populate in inflated template
        ImageView noteImage = view.findViewById(R.id.noteImage);
        TextView noteText = view.findViewById(R.id.noteText);

        // Get the image from the database as a Bitmap
        String path = cursor.getString(cursor.getColumnIndexOrThrow("noteImage"));
        Bitmap bitmap = BitmapFactory.decodeFile(path);

        // Get the note text from the database as a String
        String title = cursor.getString(cursor.getColumnIndexOrThrow("noteText"));

        String description = cursor.getString(cursor.getColumnIndexOrThrow("noteDescription"));

        String category = cursor.getString(cursor.getColumnIndexOrThrow("noteCategory"));

        // Populate fields with properties from database
        noteImage.setImageBitmap(bitmap);
        noteText.setText(title);
    }
}
```

> If you are ever missing imports from your class, you can use the shortcut Ctrl+Alt+O to automatically optimize imports. 

When inserting this code we will have some errors, that is okay. We'll address those in the next section.

> Note: We can have Android Studio auto import all required methods when initially creating a class. If we have a class that extends something and alt+enter on the erroring first line, we can import them with default values.

The constructor can be left as the default. Meanwhile, the method `newView` connects our adapter to a view we'll create in the next part of this tutorial.

Finally, the `bindView` method is where we access our database, get values, and set them to our view. When you're customizing your view, you can access elements from your `note_list_item` layout in `bindView`.

[Let's create our item view! -->](part9.html)