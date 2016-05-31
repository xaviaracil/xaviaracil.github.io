---
author: desarrolloenmac
comments: true
date: 2011-06-13 12:41:14+00:00
layout: single
slug: mostrando-mas-chicha-en-uiscrollview
title: Mostrando más "chicha" en UIScrollView
wordpress_id: 136
categories:
- Cocoa
- iOS
- Objective-C
---

La jerarquía de vistas en cocoa, ya sea AppKit como UIKit, está muy clara: cada UIView tiene una superview mientras puede tener n subviews. Todas las clases que heredan de UIView, como UIScrollView, tienen este comportamiento.

En este post acabaremos lo empezados en los dos post anteriores, modificando los UILabels por UIViews propias que, a partir de un tweet obtenido por JSON, muestren varios datos (autor, avatar y texto) y abran el Safari al hacer tap en cualquier parte de la vista.

Lo haremos en tres pasos:




  * UIView para mostrar un tweet (definido por un NSDictionary)


  * Añadir el tap para abrir Safari en la UIView


  * Añadir tantos UIView en la UIScrollView como tweets resultantes en la query JSON.


<!-- more -->


## UIView para mostrar un tweet (definido por un NSDictionary)


El primer paso es crear una UIView donde mostraremos una UIImage y dos UILabels con los datos de un NSDictionary. Siguiendo el patrón de diseño MVC, el NSDictionary sería el modelo, la nueva UIView sería la vista y haría falta un Controller para ligar los datos del Modelo con la UIView. UIKit nos da para ello la clase UIViewController.

Así pues, creamos un nuevo UIViewController en XCode con la opción New -> New File... del menú File. Seleccionamos UIViewControllerSubclass.

[![](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView1-300x204.png)](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView1.png)

Lo marcamos como subclase de UIViewController y marcamos que tenga un fichero XIB asociado.

[![](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView2-300x204.png)](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView2.png)

Le damos un nombre y seleccionamos "Save".

Abrimos el fichero XIB y colocamos una UIImage y dos UILabel, de tal forma que queden tal que así

[![](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView3.png)](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView3.png)

El UILabel llamado label está con los siguientes ajustes, de manera que ocupe 4 líneas y que ajuste el tamaño del texto para que quepa en su espacio.

[![](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView4-199x300.png)](/images/2011-06-13-mostrando-mas-chicha-en-uiscrollview/chichaUIScrollView4.png)

Falta crear los outlets para la UIImageView y los dos UILabel.


    	@property (nonatomic, readonly) IBOutlet UIImageView *photo;

    	@property (nonatomic, readonly) IBOutlet UILabel *titleLabel;

    	@property (nonatomic, readonly) IBOutlet UILabel *textLabel;


En el fichero .h añadimos la definición de aun atributo de tipo NSDictionary.


    	@property (nonatomic, retain) NSDictionary *tweetDicitonary;


Para actualizar la vista, definimos el método - (void)viewDidLoad de la siguiente manera


    	- (void)viewDidLoad {
    		[super viewDidLoad];

    		// imagen
    		NSURL *profileImageUrl = [NSURL URLWithString:[tweetDicitonary valueForKey:@"profile_image_url"]];
    		NSData *profileImageData = [NSData dataWithContentsOfURL:profileImageUrl];
    		UIImage *profileImage = [[UIImage alloc] initWithData:profileImageData];

    		self.photo.imageView.image = profileImage;

    		[profileImage release];

    		// titulo
    		self.titleLabel.text = [tweetDicitonary valueForKey:@"from_user"];

    		// texto
    		self.textLabel.text = [tweetDicitonary valueForKey:@"text"];
    	}




## Añadir el tap para abrir Safari en la UIView


Este paso es realitvamente sencillo. Esta funcionalidad se añade en el UIViewController que ya hemos creado, usando la clase UITapGestureRecognizer. Esta clase se encarga de recoger los eventos de tap y llamar a un selector dado cuando se cumplan los parámetros que le damos. Así, en nuestro caso añadimos al método -(void) viewDidLoad el siguiente trozo de código:


    	UITapGestureRecognizer *tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapAction:)];
    	tapGesture.numberOfTapsRequired = 1;
    	tapGesture.numberOfTouchesRequired = 1;
    	[self.view addGestureRecognizer:tapGesture];
    	[tapGesture release];


En la primera línea le estamos diciendo que cuando se cumplan las condiciones (definidas en las dos líneas posteriores) llame al método tapAction: de nuestra propia clase. Este método es el siguiente:


    	#pragma mark -
    	#pragma mark Tap gesture

    	- (void) tapAction:(UIGestureRecognizer *)gestureRecognizer {
    		NSString *status = [NSString stringWithFormat:@"http://www.twitter.com/%@/status/%@", [tweetDicitonary valueForKey:@"from_user"], [tweetDicitonary valueForKey:@"id"]];
    		NSURL *statusUrl = [NSURL URLWithString:status];
    		[[UIApplication sharedApplication] openURL:statusUrl];
    	}




## Añadir tantos UIView en la UIScrollView como tweets resultantes en la query JSON.


Ahora tenemos por un lado un array de NSDictionary con los resultados de la consulta JSON (ver [Obtener datos JSON]({% post_url 2011-06-08-obtener-datos-json %}) para saber cómo se obtienen) y por otro un UIViewController que muestra los datos de un NSDictionary.

El último paso es mostrar dentro del UIScrollView la vistas del UIViewController, tomando cada NSDictionary como modelo. Para ello, tendremos un NSArray donde iremos guardando los UIViewControllers.

Lo fácil es crear tantos UIViewControllers como elementos nos devuelva la llamda JSON, pero no es una buena solución en términos de rendimiento, ya que el UIScrollView sólo muestra un element, el tener 1000 en memoria no hace más que quitar recursos al resto de la aplicación.

Para inicializar el array de UIViewController's bastará añadir después de obtener los datos JSON el siguiente código


    	NSMutableArray *controllers = [[NSMutableArray alloc] init];
    	for (unsigned i = 0; i < [self.timelineArray count]; i++) {
    		[controllers addObject:[NSNull null]];
    	}
    	self.tweetViewControllers = controllers;
    	[controllers release];


Donde timelineArray es el array de NSDictionary con los tweets y tweetViewController es el NSArray con nuestros UIViewControllers. Una vez inicializado, actualizamos el contentView de la UIScrollView con un width de 200.0 píxels por el número de tweets obtenidos.


    	// set scrollview content size as appropiate
    	CGSize contentSize = CGSizeMake(200.0*[self.timelineArray count], self.searchResultsScrollView.frame.size.height);
    	[searchResultsScrollView setContentSize:contentSize];


Para después mostrar el primer y segundo elemento.


    	// load first and secong page in scroll view
    	[self loadPage:0];
    	[self loadPage:1];


La idea aquí es, dado una página, tener cargado la pagina anterior y posterior. Para ello, el código del método - (void)loadPage:(int)page es el siguiente:


    	- (void)loadPage:(int)page{
    		if (page < 0)




                            return;




                    if (page >= [self.timelineArray count])




                            return;





    		// obtener el controller del array, creandolo si es necesario
    		UIViewController *controller = [self.tweetViewControllers objectAtIndex:page];
    		if ((NSNull *)controller == [NSNull null]) {
    			controller = [[TweetViewController alloc] initWithNibName:@"TweetView" bundle:nil];
    			[self.tweetViewControllers replaceObjectAtIndex:page withObject:controller];
    			[controller release];
    		}

    		// añadir la view del controller a la scroll view
    		if (controller.view.superview == nil) {
    			CGRect frame = scrollView.frame;
    			frame.origin.x = frame.size.width * page;
    			frame.origin.y = 0;
    			controller.view.frame = frame;

    			[searchResultsScrollView addSubview:controller.view];

    			// configurar ViewController
    			NSDictionary *tweet = [self.timelineArray objectAtIndex:page];
    			controller.tweetDicitonary = tweet;
    		}
    	}


Donde TweetViewController es nuestro UIViewController y TweetView es el nombre de nuestro fichero XIB. Finalmente, implementamos el método - (void)scrollViewDidScroll:(UIScrollView *)sender del delegate del UIScrollView (el mismo controller que carga los datos JSON).


    	- (void)scrollViewDidScroll:(UIScrollView *)sender {

    		// Obtener el número de página actual
    		CGFloat pageWidth = sender.frame.size.width;
    		int page = floor((sender.contentOffset.x - pageWidth / 2) / pageWidth) + 1;

    		// cargar la página visible y la anterior y posterior
                    // para evitar bloqueos visuales al hacer scroll
    		[self loadPage:page - 1];
    		[self loadPage];
    		[self loadPage:page + 1];
    	}


Una posible optimizacion sería descargar los UIControllers no visibles


## Resumen


La clave aquí es aplicar el patrón MVC, teniendo en cuenta que una funcionalidad puede ser dada por varias vistas (UIScrollView y nuestra UIView) y varios controllers.

Post relacionados:

[Obtener datos JSON]({% post_url 2011-06-08-obtener-datos-json %})

[Uso de UIScrollView para paginar]({% post_url 2011-06-01-uso-de-uiscrollview-para-paginar %})
