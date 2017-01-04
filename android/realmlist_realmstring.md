#Create a custom RealmList

##For the Gson
~~~ java
Gson gson = new GsonBuilder()
                .setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSSZ")
                .setExclusionStrategies(new ExclusionStrategy() {
                    @Override
                    public boolean shouldSkipField(FieldAttributes f) {
                        return f.getDeclaringClass().equals(RealmObject.class);
                    }

                    @Override
                    public boolean shouldSkipClass(Class<?> clazz) {
                        return false;
                    }
                })
                .registerTypeAdapter(token, new TypeAdapter<RealmList<RealmString>>() {

                    @Override
                    public void write(JsonWriter out, RealmList<RealmString> list) throws IOException {
                        out.beginArray();
                        for (int i = 0; i < list.size(); i++) {
                            out.value(list.get(i).getValue());
                        }
                        out.endArray();
                    }

                    @Override
                    public RealmList<RealmString> read(JsonReader in) throws IOException {
                        RealmList<RealmString> list = new RealmList<>();
                        in.beginArray();
                        while (in.hasNext()) {
                            list.add(new RealmString(in.nextString()));
                        }
                        in.endArray();
                        return list;
                    }
                })
                .create();
~~~

##In the model
~~~ java
@SerializedName("available_units")
    @Expose
    private RealmList<RealmString> availableUnits = null;
~~~

##The new object for the RealmList
~~~ java
public class RealmString extends RealmObject {
    public static final String VAL_ATTR_NAME = "val";
    private String val;

    public RealmString() {
    }

    public RealmString(String val) {
        this.val = val;
    }

    public String getValue() {
        return val;
    }

    public void setValue(String value) {
        this.val = value;
    }
}
~~~

##The Realm Migration
~~~ java
//Add list of strings for units
        if (oldVersion == 2) {
            Log.i(TAG, "migrate: oldVersion: " + oldVersion);

            RealmObjectSchema realmStringSchema = schema.create(RealmString.class.getSimpleName())
                    .addField(RealmString.VAL_ATTR_NAME, String.class, FieldAttribute.REQUIRED);

            schema.get(ShoppingListItem.class.getSimpleName())
                    .addRealmListField(ShoppingListItem.AVAILABLE_UNITS_ATTR_NAME, realmStringSchema);

            oldVersion++;
        }
~~~
