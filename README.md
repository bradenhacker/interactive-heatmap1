# interactive-heatmap1
pivot_df = df.pivot_table(
    index='Category',     # Rows: App Categories
    columns='Type',       # Columns: Free vs Paid
    values='Rating',      # Values to aggregate
    aggfunc='mean'        # Use average rating as the metric
)

# Calculate the total average rating for Free and Paid apps
total_avg = df.groupby('Type')['Rating'].mean().to_frame().T
total_avg.index = ['Overall Average']

# Append the total average rating row to the pivot table
pivot_df = pd.concat([pivot_df, total_avg])

# Create an interactive heatmap using Plotly Express
fig = px.imshow(
    pivot_df,
    labels=dict(x="App Type", y="Category", color="Average Rating"),
    x=pivot_df.columns,
    y=pivot_df.index,
    color_continuous_scale='RdBu_r',
    title="Average App Rating by Category and Type (Free vs Paid)",
)

# Adjust the layout to make columns (Paid and Free) much wider
fig.update_layout(
    autosize=False,
    width=1400,  # **Significantly increased width for better visibility**
    height=950,  # **Slightly increased height to accommodate the total row**
    xaxis_title="App Type (Free vs Paid)",
    yaxis_title="Category",
)

# Display the interactive figure
fig.show()
