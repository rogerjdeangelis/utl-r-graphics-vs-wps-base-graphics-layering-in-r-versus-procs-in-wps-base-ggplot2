# utl-r-graphics-vs-wps-base-graphics-layering-in-r-versus-procs-in-wps-base-ggplot2
R graphics vs wps base graphics layering in r versus procs in wps base ggplot2 
    %let pgm=utl-r-graphics-vs-wps-base-graphics-layering-in-r-versus-procs-in-wps-base-ggplot2;

    R graphics vs wps base graphics layering in r versus procs in wps base ggplot2

    Problem:
       Add regression line to a scatter plot

    Solutions

       1 wps r
         https://stackoverflow.com/users/22737304/quinton-quagliano
       2 wps base
       3 related repos


    SOAPBOX ON

    This post demonstrates R graphics layering vs wps base proc graphics

    R uses a concept of layering to create complex graphics this nicely
    enhances wps base graphics

    SOAPBOX OFF


    Plots

    https://tinyurl.com/bde64myv
    https://github.com/rogerjdeangelis/utl-r-graphics-vs-wps-base-graphics-layering-in-r-versus-procs-in-wps-base-ggplot2/blob/main/regScatter_r.p

    https://tinyurl.com/mrdwec43
    https://github.com/rogerjdeangelis/utl-r-graphics-vs-wps-base-graphics-layering-in-r-versus-procs-in-wps-base-ggplot2/blob/main/regScatter_wps


    github
    https://tinyurl.com/26k9hae6
    https://github.com/rogerjdeangelis/utl-r-graphics-vs-wps-base-graphics-layering-in-r-versus-procs-in-wps-base-ggplot2

    stackoverflow r
    https://tinyurl.com/2v4sh2ax
    https://stackoverflow.com/questions/77293671/add-regression-manually

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*          Horsepower vs Miles Per Gallon                                                                                */
    /*                                                                                                                        */
    /*             10       20       30                                                                                       */
    /*        HP  --+--------+--------+--                                                                                     */
    /*        400 +                     + 400                                                                                 */
    /*            |                     |                                                                                     */
    /*            | \                   |                                                                                     */
    /*            |  \   *              |                                                                                     */
    /*            |   \                 |                                                                                     */
    /*        300 +    \                + 300                                                                                 */
    /*            |     \               |                                                                                     */
    /*        789 |      \  *           | 789                                                                                 */
    /*            |    ** \             |                                                                                     */
    /*            |        \            |                                                                                     */
    /*        200 +         \    *      + 200                                                                                 */
    /*            |      *  *\          |                                                                                     */
    /*            |      *    \         |                                                                                     */
    /*            |            \        |                                                                                     */
    /*            |        ** * \  *    |                                                                                     */
    /*        100 +        *  * *\*     + 100                                                                                 */
    /*            |               \     |                                                                                     */
    /*            |              *  * * |                                                                                     */
    /*            |                 \   |                                                                                     */
    /*            |                  \  |                                                                                     */
    /*          0 +                   \ +   0                                                                                 */
    /*            --+--------+--------+--                                                                                     */
    /*             10       20       30                                                                                       */
    /*                      MPG                                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input  MPG   HP ;
    cards4;
    21.0  110
    21.0  110
    22.8   93
    21.4  110
    18.7  175
    18.1  105
    14.3  245
    24.4   62
    22.8   95
    19.2  123
    17.8  123
    16.4  180
    17.3  180
    15.2  180
    10.4  205
    10.4  215
    14.7  230
    32.4   66
    30.4   52
    33.9   65
    21.5   97
    15.5  150
    15.2  150
    13.3  245
    19.2  175
    27.3   66
    26.0   91
    30.4  113
    15.8  264
    19.7  175
    15.0  335
    21.4  109
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.HAVE total obs=32                                                                                                  */
    /*                                                                                                                        */
    /* Obs     MPG     HP                                                                                                     */
    /*                                                                                                                        */
    /*   1    21.0    110                                                                                                     */
    /*   2    21.0    110                                                                                                     */
    /*   3    22.8     93                                                                                                     */
    /*  ...                                                                                                                   */
    /*  30    19.7    175                                                                                                     */
    /*  31    15.0    335                                                                                                     */
    /*  32    21.4    109                                                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*
    / | __      ___ __  ___   _ __
    | | \ \ /\ / / `_ \/ __| | `__|
    | |  \ V  V /| |_) \__ \ | |
    |_|   \_/\_/ | .__/|___/ |_|
                 |_|
    */

    %utlfkil(d:/pdf/regScatter_r.pdf);

    /*---- aes is short for aesthetics                                       ----*/

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(ggplot2);
    pdf("d:/pdf/regScatter_r.pdf");
    ggplot(
      data   = have,
      mapping = aes(
        x = MPG,
        y = HP
      )
    ) +
      geom_point() +
      geom_smooth(method = lm, se = FALSE);
    endsubmit;
    run;quit;
    ');

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* > library(ggplot2)                                                                                                     */
    /* > pdf("d:/pdf/regScatter_r.pdf")                                                                                       */
    /* > ggplot(  data   = have,  mapping = aes(    x = MPG,    y = HP  )) +                                                  */
    /*     geom_point() +  geom_smooth(method = lm, se = FALSE)                                                               */
    /* `geom_smooth()` using formula = 'y ~ x'                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                         _
    |___ \  __      ___ __  ___  | |__   __ _ ___  ___
      __) | \ \ /\ / / `_ \/ __| | `_ \ / _` / __|/ _ \
     / __/   \ V  V /| |_) \__ \ | |_) | (_| \__ \  __/
    |_____|   \_/\_/ | .__/|___/ |_.__/ \__,_|___/\___|
                     |_|
    */

    %utlfkil(d:/pdf/regScatter_wps.pdf);

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    ods pdf file="d:/pdf/regScatter_wps.pdf";
    proc sgplot data=sd1.have;
       reg y=hp x=mpg;
    run;
    ods pdf close;
    ');

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* 1         libname sd1 "d:/sd1";                                                                                        */
    /* NOTE: Library sd1 assigned as follows:                                                                                 */
    /*       Engine:        SAS7BDAT                                                                                          */
    /*       Physical Name: d:\sd1                                                                                            */
    /*                                                                                                                        */
    /* 2         ods pdf file="d:/pdf/regScatter_wps.pdf";                                                                    */
    /* 3         proc sgplot data=sd1.have;                                                                                   */
    /* 4         reg y=hp x=mpg;                                                                                              */
    /* 5         run;                                                                                                         */
    /* NOTE: Writing file d:\pdf\regScatter_wps.pdf                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*
     _ __ ___ _ __   ___  ___
    | `__/ _ \ `_ \ / _ \/ __|
    | | |  __/ |_) | (_) \__ \
    |_|  \___| .__/ \___/|___/
             |_|
    */

    https://github.com/rogerjdeangelis/utl-accessing-the-Federal-Reserve-Economic-Data-Base-and-Plotting-the-monthly-unemployment-rate
    https://github.com/rogerjdeangelis/utl-difference-in-difference-trends-plots
    https://github.com/rogerjdeangelis/utl-dropping-down-to-R-from-sas-and-creating-a-box-plot
    https://github.com/rogerjdeangelis/utl-flexible-methods-for-labeling-points-on-a-scatter-plot
    https://github.com/rogerjdeangelis/utl-graphics-boxplots-with-jiggled-point-values-alongside
    https://github.com/rogerjdeangelis/utl-meta-analysis-of-a-single-proportion-in-R-with-forest-and-funnel-plots
    https://github.com/rogerjdeangelis/utl-meta-analysis-of-deaths-in-twenty-studies-with-forest-plot
    https://github.com/rogerjdeangelis/utl-optimum-number-of-clusters-elbow-plot
    https://github.com/rogerjdeangelis/utl-overlaying-histograms-and-scatterplots-in-powerpoint-pdf-and-jpeg
    https://github.com/rogerjdeangelis/utl-plot-eighteen-random-points-within-Nova-Scotia
    https://github.com/rogerjdeangelis/utl-plot-of-the-rivers-in-brazil-using-public-shapefiles
    https://github.com/rogerjdeangelis/utl-sgplot-labeling-vertical-barcharts-with-percentages
    https://github.com/rogerjdeangelis/utl_3D_scatter_plots_in_SAS_Python_and_R
    https://github.com/rogerjdeangelis/utl_R_graphics_polar_plot
    https://github.com/rogerjdeangelis/utl_classic_sas_graph_greplay_harvard_macro_multiple_plots_per_page
    https://github.com/rogerjdeangelis/utl_classic_sas_graph_three_plots_across_many_methods_long_live
    https://github.com/rogerjdeangelis/utl_convex_hull_maximum-distance-between-two-points-in-a-scatter-plot
    https://github.com/rogerjdeangelis/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot
    https://github.com/rogerjdeangelis/utl_given_the_latitude_and_longitude_box_plot_a_country
    https://github.com/rogerjdeangelis/utl_graphics_fit_a_smooth_line_to_a_scatter_plot_loess
    https://github.com/rogerjdeangelis/utl_graphics_plotting_rivers_in_brazil_using_sharpefiles
    https://github.com/rogerjdeangelis/utl_meta_analysis_funnel_plot_odds_ratio_vs_standard_error
    https://github.com/rogerjdeangelis/utl_plot_with_two_different_x_axis_for_the_same_variable_in_r
    https://github.com/rogerjdeangelis/utl_polar_graphics_pot_violin_plot
    https://github.com/rogerjdeangelis/utl_sas_classic_graphics_15_plots_on_a_page
    https://github.com/rogerjdeangelis/utl_simple_convex_hull_polygon_envelop_for_a_scatter_plot
    https://github.com/rogerjdeangelis/utl_ternary_plots_for_US_2016_election
    https://github.com/rogerjdeangelis/utl_visualizing_suspicious_bivariate_outliers_with_2_dimensional_boxplots


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
