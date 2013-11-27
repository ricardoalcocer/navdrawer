# Proposal

The module should be attached to the app's main window in the open event, just like the ActionBar.

     var navdrawer = require('com.alcoapps.navigationdrawer');

     var win1 = Titanium.UI.createWindow({  
     	title:'Main Window',
		backgroundColor:'#fff',
    	navBarHidden: false
     });

     // this is the menu
     var tv=Ti.UI.createTableView({
     	data:[{title:"row1"},{title:"row2"}]
     })

     win1.addEventListener('open',function(e){
     	// call the module sending the menu (tv)
	    navdrawer.attach(tv);
     })

     win1.open();
     
     
The module should receive the TableView, which will then be added the the DrawerLayout.

I'm using [this](https://github.com/ricardoalcocer/AndroidNavigationDrawer/blob/master/src/com/javatechig/drawer/MainActivity.java) as a base.

Based on that code, here's some pseudo-code to implement the [Drawer Layout Widget](http://developer.android.com/design/patterns/navigation-drawer.html).

     // Within which the entire activity is enclosed
     private DrawerLayout mDrawerLayout;

     // ActionBarDrawerToggle indicates the presence of Navigation Drawer in the action bar
     private ActionBarDrawerToggle mDrawerToggle;


     // Getting reference to the DrawerLayout
     mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);

     mDrawerList = // this is the tableview received in the attach method

     // Getting reference to the ActionBarDrawerToggle
     mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout,
                R.drawable.ic_drawer, R.string.drawer_open,
                R.string.drawer_close) {

        /** Called when drawer is closed */
        public void onDrawerClosed(View view) {
                getActionBar().setTitle(mTitle);
                invalidateOptionsMenu();

        }

        /** Called when a drawer is opened */
        public void onDrawerOpened(View drawerView) {
                getActionBar().setTitle("JAVATECHIG.COM");
                invalidateOptionsMenu();
        }

     };

     // Setting DrawerToggle on DrawerLayout
     mDrawerLayout.setDrawerListener(mDrawerToggle);

     // Enabling Home button
     getActionBar().setHomeButtonEnabled(true);

     // Enabling Up navigation
     getActionBar().setDisplayHomeAsUpEnabled(true);