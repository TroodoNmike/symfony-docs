.. index::
   pair: Twig; Configuration reference

TwigBundle Configuration Reference
==================================

.. configuration-block::

    .. code-block:: yaml

        twig:
            form:
                resources:

                    # Default:
                    - form_div_layout.html.twig

                    # Example:
                    - MyBundle::form.html.twig
            globals:

                # Examples:
                foo:                 "@bar"
                pi:                  3.14

                # Example options, but the easiest use is as seen above
                some_variable_name:
                    # a service id that should be the value
                    id:                   ~
                    # set to service or leave blank
                    type:                 ~
                    value:                ~
            autoescape:           ~
            base_template_class:  ~ # Example: Twig_Template
            cache:                "%kernel.cache_dir%/twig"
            charset:              "%kernel.charset%"
            debug:                "%kernel.debug%"
            strict_variables:     ~
            auto_reload:          ~
            optimizations:        ~

    .. code-block:: xml

        <container xmlns="http://symfony.com/schema/dic/services"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:twig="http://symfony.com/schema/dic/twig"
            xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd
                                http://symfony.com/schema/dic/twig http://symfony.com/schema/dic/doctrine/twig-1.0.xsd">

            <twig:config auto-reload="%kernel.debug%" autoescape="true" base-template-class="Twig_Template" cache="%kernel.cache_dir%/twig" charset="%kernel.charset%" debug="%kernel.debug%" strict-variables="false">
                <twig:form>
                    <twig:resource>MyBundle::form.html.twig</twig:resource>
                </twig:form>
                <twig:global key="foo" id="bar" type="service" />
                <twig:global key="pi">3.14</twig:global>
            </twig:config>
        </container>

    .. code-block:: php

        $container->loadFromExtension('twig', array(
            'form' => array(
                'resources' => array(
                    'MyBundle::form.html.twig',
                )
             ),
             'globals' => array(
                 'foo' => '@bar',
                 'pi'  => 3.14,
             ),
             'auto_reload'         => '%kernel.debug%',
             'autoescape'          => true,
             'base_template_class' => 'Twig_Template',
             'cache'               => '%kernel.cache_dir%/twig',
             'charset'             => '%kernel.charset%',
             'debug'               => '%kernel.debug%',
             'strict_variables'    => false,
        ));

Configuration
-------------

.. _config-twig-exception-controller:

exception_controller
....................

**type**: ``string`` **default**: ``Symfony\\Bundle\\TwigBundle\\Controller\\ExceptionController::showAction``

This is the controller that is activated after an exception is thrown anywhere
in your application. The default controller
(:class:`Symfony\\Bundle\\TwigBundle\\Controller\\ExceptionController`)
is what's responsible for rendering specific templates under different error
conditions (see :doc:`/cookbook/controller/error_pages`). Modifying this
option is advanced. If you need to customize an error page you should use
the previous link. If you need to perform some behavior on an exception,
you should add a listener to the ``kernel.exception`` event (see :ref:`dic-tags-kernel-event-listener`).

.. versionadded:: 2.2
    moved the exception controller to be a service (`twig.controller.exception:showAction` vs `Symfony\\Bundle\\TwigBundle\\Controller\\ExceptionController::showAction`)


.. configuration-block::

    .. code-block:: yaml

        # app/config/config.yml
        parameters:
            twig.controller.exception.class:      Symfony\Bundle\TwigBundle\Controller\ExceptionController

        services:
            twig.controller.exception:
                class:        "%twig.controller.exception.class%"
                arguments:    [@twig, %kernel.debug%]

    .. code-block:: xml

        <!-- app/config/config.xml -->
        <parameters>
            <parameter key="twig.controller.exception.class">Symfony\Bundle\TwigBundle\Controller\ExceptionController</parameter>
        </parameters>

        <service id="twig.controller.exception" class="%twig.controller.exception.class%">
            <argument type="service" id="twig" />
            <argument>%kernel.debug%</argument>
        </service>

    .. code-block:: php

        // app/config/config.php
        use Symfony\Component\DependencyInjection\Definition;

        $container->setParameter('twig.controller.exception.class', 'Symfony\Bundle\TwigBundle\Controller\ExceptionController');

        $container->setDefinition('twig.controller.exception', new Definition(
            '%twig.controller.exception.class%',
            array('@twig', '%kernel.debug%')
        ));