import dash
from dash import dcc, html
import plotly.graph_objects as go
import plotly.express as px
import pandas as pd
import numpy as np

# Initialize the Dash app
app = dash.Dash(__name__, meta_tags=[{"name": "viewport", "content": "width=device-width, initial-scale=1"}])
app.title = "Business Development Portfolio Dashboard"

# Create sample data for the charts
months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']
sales_growth = [2, 5, 8, 11, 13, 15]
customer_satisfaction = [3, 7, 11, 16, 21, 25]
waste_reduction = [1, 5, 8, 12, 16, 20]

df_trends = pd.DataFrame({
    'Month': months,
    'Sales Growth (%)': sales_growth,
    'Customer Satisfaction (%)': customer_satisfaction,
    'Waste Reduction (%)': waste_reduction
})

# Creating a color palette
colors = {
    'background': '#f5f7fa',
    'card': 'white',
    'header': 'linear-gradient(135deg, #2c3e50, #4ca1af)',
    'text': '#333333',
    'sales': '#3498db',
    'satisfaction': '#e74c3c',
    'waste': '#2ecc71',
    'border': '#e0e0e0'
}

# Define the layout of the app
app.layout = html.Div(
    style={'backgroundColor': colors['background'], 'fontFamily': 'Segoe UI, Tahoma, Geneva, Verdana, sans-serif'},
    children=[
        # Header
        html.Div(
            style={
                'textAlign': 'center',
                'padding': '20px',
                'background': colors['header'],
                'color': 'white',
                'borderRadius': '10px',
                'margin': '20px 20px 30px 20px',
                'boxShadow': '0 4px 6px rgba(0,0,0,0.1)'
            },
            children=[
                html.H1("Business Development Associate Portfolio", style={'marginBottom': '10px'}),
                html.P("Driving Growth Through Data-Driven Strategies", style={'fontSize': '18px'})
            ]
        ),
        
        # Profile Section
        html.Div(
            style={
                'display': 'flex',
                'flexDirection': 'row',
                'alignItems': 'center',
                'justifyContent': 'space-between',
                'backgroundColor': colors['card'],
                'padding': '20px',
                'margin': '0 20px 30px 20px',
                'borderRadius': '10px',
                'boxShadow': '0 2px 4px rgba(0,0,0,0.05)'
            },
            children=[
                html.Div(
                    style={'width': '150px', 'height': '150px', 'backgroundColor': '#e0e0e0', 'borderRadius': '50%', 'display': 'flex', 'alignItems': 'center', 'justifyContent': 'center', 'marginRight': '20px'},
                    children=[
                        html.Img(src="assets/profile_placeholder.png", style={'width': '100%', 'height': '100%', 'borderRadius': '50%', 'objectFit': 'cover'})
                    ]
                ),
                html.Div(
                    style={'flex': '2'},
                    children=[
                        html.H2("Business Development Associate"),
                        html.P("Specialized in leveraging data analytics to drive business growth, enhance customer experience, and optimize operational efficiency. Demonstrated success in implementing innovative solutions that directly impact sales performance and customer satisfaction.")
                    ]
                )
            ]
        ),
        
        # Key Performance Metrics
        html.H2("Key Performance Highlights", style={'margin': '0 20px 20px 20px', 'color': '#2c3e50', 'borderBottom': '2px solid #4ca1af', 'paddingBottom': '10px'}),
        html.Div(
            style={
                'display': 'grid',
                'gridTemplateColumns': 'repeat(3, 1fr)',
                'gap': '20px',
                'margin': '0 20px 30px 20px'
            },
            children=[
                # Sales Growth
                html.Div(
                    style={
                        'backgroundColor': colors['card'],
                        'padding': '20px',
                        'borderRadius': '10px',
                        'textAlign': 'center',
                        'boxShadow': '0 2px 4px rgba(0,0,0,0.05)',
                        'transition': 'transform 0.3s ease',
                        ':hover': {'transform': 'translateY(-5px)', 'boxShadow': '0 4px 8px rgba(0,0,0,0.1)'}
                    },
                    children=[
                        html.H3("Sales Growth"),
                        html.Div("+15%", style={'fontSize': '2.5rem', 'fontWeight': 'bold', 'color': '#2c3e50', 'margin': '10px 0'}),
                        html.P("Increase in quarterly sales through optimized data pipelines and improved forecasting")
                    ]
                ),
                # Waste Reduction
                html.Div(
                    style={
                        'backgroundColor': colors['card'],
                        'padding': '20px',
                        'borderRadius': '10px',
                        'textAlign': 'center',
                        'boxShadow': '0 2px 4px rgba(0,0,0,0.05)',
                        'transition': 'transform 0.3s ease'
                    },
                    children=[
                        html.H3("Waste Reduction"),
                        html.Div("20%", style={'fontSize': '2.5rem', 'fontWeight': 'bold', 'color': '#2c3e50', 'margin': '10px 0'}),
                        html.P("Decrease in inventory waste through implementation of predictive modeling")
                    ]
                ),
                # Customer Satisfaction
                html.Div(
                    style={
                        'backgroundColor': colors['card'],
                        'padding': '20px',
                        'borderRadius': '10px',
                        'textAlign': 'center',
                        'boxShadow': '0 2px 4px rgba(0,0,0,0.05)',
                        'transition': 'transform 0.3s ease'
                    },
                    children=[
                        html.H3("Customer Satisfaction"),
                        html.Div("+25%", style={'fontSize': '2.5rem', 'fontWeight': 'bold', 'color': '#2c3e50', 'margin': '10px 0'}),
                        html.P("Improvement in satisfaction scores through enhanced feedback systems")
                    ]
                )
            ]
        ),
        
        # Performance Trends Chart
        html.Div(
            style={
                'backgroundColor': colors['card'],
                'padding': '20px',
                'margin': '0 20px 30px 20px',
                'borderRadius': '10px',
                'boxShadow': '0 2px 4px rgba(0,0,0,0.05)'
            },
            children=[
                html.H2("Performance Trends", style={'marginBottom': '20px', 'color': '#2c3e50', 'borderBottom': '2px solid #4ca1af', 'paddingBottom': '10px'}),
                dcc.Graph(
                    figure=go.Figure(
                        data=[
                            go.Scatter(
                                x=months,
                                y=sales_growth,
                                mode='lines+markers',
                                name='Sales Growth',
                                line=dict(color=colors['sales'], width=3),
                                marker=dict(size=8)
                            ),
                            go.Scatter(
                                x=months,
                                y=customer_satisfaction,
                                mode='lines+markers',
                                name='Customer Satisfaction',
                                line=dict(color=colors['satisfaction'], width=3, dash='dash'),
                                marker=dict(size=8)
                            ),
                            go.Scatter(
                                x=months,
                                y=waste_reduction,
                                mode='lines+markers',
                                name='Waste Reduction',
                                line=dict(color=colors['waste'], width=3, dash='dot'),
                                marker=dict(size=8)
                            )
                        ],
                        layout=go.Layout(
                            height=400,
                            margin=dict(l=40, r=40, t=40, b=40),
                            legend=dict(
                                orientation="h",
                                yanchor="bottom",
                                y=1.02,
                                xanchor="right",
                                x=1
                            ),
                            xaxis=dict(
                                title='Month',
                                showgrid=True,
                                gridcolor='#e0e0e0'
                            ),
                            yaxis=dict(
                                title='Percentage (%)',
                                showgrid=True,
                                gridcolor='#e0e0e0'
                            ),
                            plot_bgcolor='rgba(0,0,0,0)',
                            paper_bgcolor='rgba(0,0,0,0)'
                        )
                    )
                )
            ]
        ),
        
        # Key Projects Section
        html.H2("Key Projects", style={'margin': '0 20px 20px 20px', 'color': '#2c3e50', 'borderBottom': '2px solid #4ca1af', 'paddingBottom': '10px'}),
        html.Div(
            style={'margin': '0 20px 30px 20px'},
            children=[
                # Project 1
                html.Div(
                    style={
                        'backgroundColor': colors['card'],
                        'padding': '20px',
                        'borderRadius': '10px',
                        'boxShadow': '0 2px 4px rgba(0,0,0,0.05)',
                        'marginBottom': '20px'
                    },
                    children=[
                        html.H3(
                            children=[
                                html.Div(
                                    "1",
                                    style={
                                        'marginRight': '10px',
                                        'width': '24px',
                                        'height': '24px',
                                        'backgroundColor': '#4ca1af',
                                        'borderRadius': '50%',
                                        'display': 'inline-block',
                                        'textAlign': 'center',
                                        'color': 'white',
                                        'lineHeight': '24px'
                                    }
                                ),
                                "Sales Forecasting Data Pipeline"
                            ],
                            style={'display': 'flex', 'alignItems': 'center', 'color': '#2c3e50', 'marginBottom': '10px'}
                        ),
                        html.P("Designed and implemented data pipelines to improve sales forecasting accuracy. Integrated multiple data sources to create a comprehensive view of market trends and customer behaviors, enabling more accurate revenue projections."),
                        html.Ul(
                            style={'marginTop': '10px', 'marginLeft': '20px'},
                            children=[
                                html.Li("Integrated CRM data with external market indicators"),
                                html.Li("Developed automated reporting dashboards for real-time insights"),
                                html.Li("Created early warning system for sales pipeline issues"),
                                html.Li(html.Strong("Result: 15% increase in sales within three months"))
                            ]
                        )
                    ]
                ),
                # Project 2
                html.Div(
                    style={
                        'backgroundColor': colors['card'],
                        'padding': '20px',
                        'borderRadius': '10px',
                        'boxShadow': '0 2px 4px rgba(0,0,0,0.05)',
                        'marginBottom': '20px'
                    },
                    children=[
                        html.H3(
                            children=[
                                html.Div(
                                    "2",
                                    style={
                                        'marginRight': '10px',
                                        'width': '24px',
                                        'height': '24px',
                                        'backgroundColor': '#4ca1af',
                                        'borderRadius': '50%',
                                        'display': 'inline-block',
                                        'textAlign': 'center',
                                        'color': 'white',
                                        'lineHeight': '24px'
                                    }
                                ),
                                "Inventory Optimization System"
                            ],
                            style={'display': 'flex', 'alignItems': 'center', 'color': '#2c3e50', 'marginBottom': '10px'}
                        ),
                        html.P("Automated inventory management processes using predictive modeling to anticipate demand fluctuations and optimize stock levels. Implemented machine learning algorithms to identify patterns in historical data and external factors affecting inventory requirements."),
                        html.Ul(
                            style={'marginTop': '10px', 'marginLeft': '20px'},
                            children=[
                                html.Li("Created demand forecasting models based on seasonal patterns"),
                                html.Li("Implemented automatic reordering thresholds"),
                                html.Li("Developed supplier performance metrics"),
                                html.Li(html.Strong("Result: 20% reduction in inventory waste"))
                            ]
                        )
                    ]
                ),
                # Project 3
                html.Div(
                    style={
                        'backgroundColor': colors['card'],
                        'padding': '20px',
                        'borderRadius': '10px',
                        'boxShadow': '0 2px 4px rgba(0,0,0,0.05)',
                        'marginBottom': '20px'
                    },
                    children=[
                        html.H3(
                            children=[
                                html.Div(
                                    "3",
                                    style={
                                        'marginRight': '10px',
                                        'width': '24px',
                                        'height': '24px',
                                        'backgroundColor': '#4ca1af',
                                        'borderRadius': '50%',
                                        'display': 'inline-block',
                                        'textAlign': 'center',
                                        'color': 'white',
                                        'lineHeight': '24px'
                                    }
                                ),
                                "Customer Feedback Enhancement"
                            ],
                            style={'display': 'flex', 'alignItems': 'center', 'color': '#2c3e50', 'marginBottom': '10px'}
                        ),
                        html.P("Developed comprehensive customer feedback systems to capture and analyze customer sentiments across multiple touchpoints. Implemented sentiment analysis tools to categorize feedback and identify key areas for improvement."),
                        html.Ul(
                            style={'marginTop': '10px', 'marginLeft': '20px'},
                            children=[
                                html.Li("Created omnichannel feedback collection system"),
                                html.Li("Implemented real-time sentiment analysis dashboard"),
                                html.Li("Developed action plan templates for addressing negative feedback"),
                                html.Li(html.Strong("Result: 25% improvement in customer satisfaction scores"))
                            ]
                        )
                    ]
                )
            ]
        ),
        
        # Skills Section
        html.H2("Skills & Expertise", style={'margin': '0 20px 20px 20px', 'color': '#2c3e50', 'borderBottom': '2px solid #4ca1af', 'paddingBottom': '10px'}),
        html.Div(
            style={
                'backgroundColor': colors['card'],
                'padding': '20px',
                'margin': '0 20px 30px 20px',
                'borderRadius': '10px',
                'boxShadow': '0 2px 4px rgba(0,0,0,0.05)'
            },
            children=[
                html.Div(
                    style={
                        'display': 'grid',
                        'gridTemplateColumns': 'repeat(4, 1fr)',
                        'gap': '15px',
                        'marginTop': '20px'
                    },
                    children=[
                        html.Div("Data Analysis", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Sales Forecasting", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Predictive Modeling", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("CRM Systems", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Business Intelligence", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Market Research", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Project Management", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Customer Insights", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("SQL", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Python", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Tableau", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'}),
                        html.Div("Power BI", style={'backgroundColor': '#f0f4f8', 'padding': '10px', 'borderRadius': '5px', 'textAlign': 'center', 'fontWeight': '500'})
                    ]
                )
            ]
        ),
        
        # Footer
        html.Div(
            style={
                'textAlign': 'center',
                'marginTop': '50px',
                'marginBottom': '20px',
                'color': '#666',
                'fontSize': '0.9rem'
            },
            children=[
                html.P("© 2025 Business Development Portfolio | Updated March 2025")
            ]
        )
    ]
)

# Run the app
if __name__ == '__main__':
    app.run_server(debug=True)
