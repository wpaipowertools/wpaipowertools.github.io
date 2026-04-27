---
layout: default
slug: ai-admin-assistance
menu: user
title: Setup for Plugin and Theme Authors
---
Using the AI Admin Assistance panel to display help content for your plugin or theme is a straightforward process with a few steps. Here we'll cover these in enough detail to get you up and running smoothly and quickly!

In our example below, we have created a class called `mypluginHelper` specifically for the AI Admin Assistance integration and will be using methods in this class to create the integration and help content. 

## Where do I add the code?

This is mostly a matter of preference. Traditionally, if creating a **theme**, you might place such functions in your `functions.php` file. And, if creating a **plugin**, you'd have more flexibility. For organizational reasons, you may want to create a new file for the new class (perhaps in an `includes` folder you may already have). The important thing is that, wherever you place the code, you make sure the class/code is loaded at least on all the admin pages on which you would like to display the panel/help content.

## Integration

First, add a filter that runs a function where we will create the help content.

```
public static function aiaa_add_filter() {

    add_filter( 'ait_aiaa_third_party_information', array( __CLASS__, 'add_help_to_aiaa' ), 20, 2 );
}
```

Here you can see that it refers to the `add_help_to_aiaa` method in the same class.

## Adding the actual help content

Here is where we will add the actual content that will display in the AI Admin Assistance panel on your targeted pages/screens.

```
public static function add_help_to_aiaa( $items, $context ) {

    $items   = is_array( $items ) ? $items : array();
    $context = is_array( $context ) ? $context : array();

    $screen_id = isset( $context['screen_id'] ) ? (string) $context['screen_id'] : '';
    $post_type = isset( $context['post_type'] ) ? (string) $context['post_type'] : '';
    $taxonomy  = isset( $context['taxonomy'] ) ? (string) $context['taxonomy'] : '';

    if ( ! self::aiaa_matches_context( $screen_id, $post_type, $taxonomy ) ) { return $items; }

    $page_details = self::get_page_details_for_context( $context );

    $tutorial_links = array();
    if ( ! empty( $page_details['tutorials'] ) && is_array( $page_details['tutorials'] ) ) {

      foreach ( $page_details['tutorials'] as $tutorial ) {

        if ( empty( $tutorial['url'] ) || empty( $tutorial['title'] ) ) { continue; }

        $tutorial_links[] = array(
          'title' => (string) $tutorial['title'],
          'url'   => (string) $tutorial['url'],
        );
      }
    }

    $general_links = array();
    if ( ! empty( self::$documentation_link ) ) {
      $general_links[] = array(
        'title' => __( 'Documentation', 'food-and-drink-menu' ),
        'url'   => self::$documentation_link,
      );
    }
    if ( ! empty( self::$tutorials_link ) ) {
      $general_links[] = array(
        'title' => __( 'YouTube Tutorials', 'food-and-drink-menu' ),
        'url'   => self::$tutorials_link,
      );
    }
    if ( ! empty( self::$faq_link ) ) {
      $general_links[] = array(
        'title' => __( 'FAQ', 'food-and-drink-menu' ),
        'url'   => self::$faq_link,
      );
    }
    if ( ! empty( self::$wp_forum_support_link ) ) {
      $general_links[] = array(
        'title' => __( 'WP Forum Support', 'food-and-drink-menu' ),
        'url'   => self::$wp_forum_support_link,
      );
    }
    if ( ! empty( self::$support_center_link ) ) {
      $general_links[] = array(
        'title' => __( 'Support Center', 'food-and-drink-menu' ),
        'url'   => self::$support_center_link,
      );
    }

    $help_links = array();
    if ( ! empty( $tutorial_links ) ) { $help_links[ __( 'Tutorials', 'food-and-drink-menu' ) ] = $tutorial_links; }
    if ( ! empty( $general_links ) ) { $help_links[ __( 'General', 'food-and-drink-menu' ) ] = $general_links; }

    $items[] = array(
      'id'              => 'fdm_help',
      'title'           => __( 'Restaurant Menu Help', 'food-and-drink-menu' ),
      'description'     => ! empty( $page_details['description'] ) ? '<p>' . esc_html( $page_details['description'] ) . '</p>' : '',
      'help_links'      => $help_links,
      'source'          => array(
        'type' => 'plugin',
        'name' => 'Five Star Restaurant Menu and Food Ordering',
        'slug' => 'food-and-drink-menu',
      ),
      'target_callback' => array( __CLASS__, 'aiaa_target_callback' ),
      'priority'        => 20,
      'capability'      => 'manage_options',
      'icon'            => 'dashicons-editor-help',
    );

    return $items;
  }
```

## Setting where to show your help content

The above method created the help content to be displayed in the AI Admin Assistance panel. You'll notice that it references `aiaa_matches_context`. This is a separate method that sets where to show that content. An example of how this might look is:

```
private static function aiaa_matches_context( $screen_id, $post_type, $taxonomy ) {

    if ( ! empty( $post_type ) && in_array( $post_type, self::$post_types, true ) ) { return true; }

    if ( ! empty( $taxonomy ) && in_array( $taxonomy, self::$taxonomies, true ) ) { return true; }

    if ( ! empty( $screen_id ) && ! empty( self::$additional_pages ) ) {

        foreach ( self::$additional_pages as $slug ) {

            if ( empty( $slug ) ) { continue; }

            if ( strpos( $screen_id, $slug ) !== false ) { return true; }
        }
    }

    return false;
  }
```

You may have also noticed thast this method and `add_help_to_aiaa` both also reference `aiaa_target_callback`. This last method helps bring everything together. An example of this would be:

```
public static function aiaa_target_callback( $context, $item ) {

    $context = is_array( $context ) ? $context : array();

    $screen_id = isset( $context['screen_id'] ) ? (string) $context['screen_id'] : '';
    $post_type = isset( $context['post_type'] ) ? (string) $context['post_type'] : '';
    $taxonomy  = isset( $context['taxonomy'] ) ? (string) $context['taxonomy'] : '';

    return self::aiaa_matches_context( $screen_id, $post_type, $taxonomy );
}
```

## Final piece

The final piece in the puzzle is making sure you register the action to make the integration happen. This would look like:

```
add_action( 'plugins_loaded', array( 'mypluginHelper', 'aiaa_add_filter' ), 20 );
```

Where `mypluginHelper` is the name of our class and calls our original `aiaa_add_filter` method.